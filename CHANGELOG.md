## 0.5.0 (unreleased)

FEATURES:

  * **Multi-provider (a.k.a multi-region)**: Multiple instances of a single
     provider can be configured so resources can apply to different settings.
     As an example, this allows Terraform to manage multiple regions with AWS.
  * **Environmental variables to set variables**: Environment variables can be
     used to set variables. The environment variables must be in the format
     `TF_VAR_name` and this will be checked last for a value.

IMPROVEMENTS:

  * **New config function: `length`** - Get the length of a string or a list.
      Useful in conjunction with `split`. [GH-1495]
  * **New resource: `aws_app_cookie_stickiness_policy`**
  * **New resource: `aws_ebs_volume`**
  * **New resource: `aws_lb_cookie_stickiness_policy`**
  * core: Improve error message on diff mismatch [GH-1501]
  * provisioner/file: expand `~` in source path [GH-1569]
  * provider/aws: Can specify a `token` via the config file [GH-1601]
  * provider/aws: White or blacklist account IDs that can be used to
      protect against accidents. [GH-1595]
  * provider/aws: `aws_instance` supports placement groups [GH-1358]
  * provider/aws: `aws_elb` supports in-place changing of listeners [GH-1619]
  * provider/aws: `aws_elb` supports connection draining settings [GH-1502]
  * provider/aws: `aws_elb` increase default idle timeout to 60s [GH-1646]
  * provider/aws: `aws_route53_record` supports weighted sets [GH-1578]
  * provider/aws: `aws_route53_zone` exports nameservers [GH-1525]
  * provider/aws: `aws_security_group` name becomes optional and can be
      automatically set to a unique identifier; this helps with
      `create_before_destroy` scenarios [GH-1632]
  * provider/aws: `aws_security_group` description becomes optional with a
      static default value [GH-1632]
  * provider/aws: automatically set the private IP as the SSH address
      if not specified and no public IP is available [GH-1623]
  * provider/docker: `docker_container` can specify links [GH-1564]
  * provider/google: `resource_compute_disk` supports snapshots [GH-1426]
  * provider/google: `resource_compute_instance` supports specifying the
      device name [GH-1426]
  * provider/openstack: Floating IP support for LBaaS [GH-1550]
  * provider/openstack: Add AZ to `openstack_blockstorage_volume_v1` [GH-1726]

BUG FIXES:

  * core: math on arbitrary variables works if first operand isn't a
      numeric primitive. [GH-1381]
  * core: avoid unnecessary cycles by pruning tainted destroys from
      graph if there are no tainted resources [GH-1475]
  * core: fix issue where destroy nodes weren't pruned in specific
      edge cases around matching prefixes, which could cause cycles [GH-1527]
  * core: fix issue causing diff mismatch errors in certain scenarios during
      resource replacement [GH-1515]
  * core: dependencies on resources with a different index work when
      count > 1 [GH-1540]
  * core: don't panic if variable default type is invalid [GH-1344]
  * core: fix perpetual diff issue for computed maps that are empty [GH-1607]
  * core: validation added to check for `self` variables in modules [GH-1609]
  * core: fix edge case where validation didn't pick up unknown fields
      if the value was computed [GH-1507]
  * core: Fix issue where values in sets on resources couldn't contain
      hyphens. [GH-1641]
  * command: remote states with uppercase types work [GH-1356]
  * provider/aws: launch configuration ID set after create success [GH-1518]
  * provider/aws: manually deleted S3 buckets are refreshed properly [GH-1574]
  * provider/aws: only check for EIP allocation ID in VPC [GH-1555]
  * provider/aws: raw protocol numbers work in `aws_network_acl` [GH-1435]
  * provider/aws: Block devices can be encrypted [GH-1718]
  * provider/aws: ASG health check grace period can be updated in-place [GH-1682]
  * provider/aws: ELB security groups can be updated in-place [GH-1662]
  * provider/openstack: region config is not required [GH-1441]
  * provisioner/remote-exec: add random number to uploaded script path so
      that parallel provisions work [GH-1588]

## 0.4.2 (April 10, 2015)

BUG FIXES:

  * core: refresh won't remove outputs from state file [GH-1369]
  * core: clarify "unknown variable" error [GH-1480]
  * core: properly merge parent provider configs when asking for input
  * provider/aws: fix panic possibility if RDS DB name is empty [GH-1460]
  * provider/aws: fix issue detecting credentials for some resources [GH-1470]
  * provider/google: fix issue causing unresolvable diffs when using legacy
      `network` field on `google_compute_instance` [GH-1458]

## 0.4.1 (April 9, 2015)

IMPROVEMENTS:

  * provider/aws: Route 53 records can now update `ttl` and `records` attributes
      without destroying/creating the record [GH-1396]
  * provider/aws: Support changing additional attributes of RDS databases
      without forcing a new resource  [GH-1382]

BUG FIXES:

  * core: module paths in ".terraform" are consistent across different
      systems so copying your ".terraform" folder works. [GH-1418]
  * core: don't validate providers too early when nested in a module [GH-1380]
  * core: fix race condition in `count.index` interpolation [GH-1454]
  * command/push: don't ask for input if terraform.tfvars is present
  * command/remote-config: remove spurrious error "nil" when initializing
      remote state on a new configuration. [GH-1392]
  * provider/aws: Fix issue with Route 53 and pre-existing Hosted Zones [GH-1415]
  * provider/aws: Fix refresh issue in Route 53 hosted zone [GH-1384]
  * provider/aws: Fix issue when changing map-public-ip in Subnets #1234
  * provider/aws: Fix issue finding db subnets [GH-1377]
  * provider/aws: Fix issues with `*_block_device` attributes on instances and
      launch configs creating unresolvable diffs when certain optional
      parameters were omitted from the config [GH-1445]
  * provider/aws: Fix issue with `aws_launch_configuration` causing an
      unnecessary diff for pre-0.4 environments [GH-1371]
  * provider/aws: Fix several related issues with `aws_launch_configuration`
      causing unresolvable diffs [GH-1444]
  * provider/aws: Fix issue preventing launch configurations from being valid
      in EC2 Classic [GH-1412]
  * provider/aws: Fix issue in updating Route 53 records on refresh/read. [GH-1430]
  * provider/docker: Don't ask for `cert_path` input on every run [GH-1432]
  * provider/google: Fix issue causing unresolvable diff on instances with
      `network_interface` [GH-1427]

## 0.4.0 (April 2, 2015)

BACKWARDS INCOMPATIBILITIES:

  * Commands `terraform push` and `terraform pull` are now nested under
    the `remote` command: `terraform remote push` and `terraform remote pull`.
    The old `remote` functionality is now at `terraform remote config`. This
    consolidates all remote state management under one command.
  * Period-prefixed configuration files are now ignored. This might break
    existing Terraform configurations if you had period-prefixed files.
  * The `block_device` attribute of `aws_instance` has been removed in favor
    of three more specific attributes to specify block device mappings:
    `root_block_device`, `ebs_block_device`, and `ephemeral_block_device`.
    Configurations using the old attribute will generate a validation error
    indicating that they must be updated to use the new fields [GH-1045].

FEATURES:

  * **New provider: `dme` (DNSMadeEasy)** [GH-855]
  * **New provider: `docker` (Docker)** - Manage container lifecycle
      using the standard Docker API. [GH-855]
  * **New provider: `openstack` (OpenStack)** - Interact with the many resources
      provided by OpenStack. [GH-924]
  * **New feature: `terraform_remote_state` resource** - Reference remote
      states from other Terraform runs to use Terraform outputs as inputs
      into another Terraform run.
  * **New command: `taint`** - Manually mark a resource as tainted, causing
      a destroy and recreate on the next plan/apply.
  * **New resource: `aws_vpn_gateway`** [GH-1137]
  * **New resource: `aws_elastic_network_interfaces`** [GH-1149]
  * **Self-variables** can be used to reference the current resource's
      attributes within a provisioner. Ex. `${self.private_ip_address}` [GH-1033]
  * **Continuous state** saving during `terraform apply`. The state file is
      continuously updated as apply is running, meaning that the state is
      less likely to become corrupt in a catastrophic case: terraform panic
      or system killing Terraform.
  * **Math operations** in interpolations. You can now do things like
      `${count.index+1}`. [GH-1068]
  * **New AWS SDK:** Move to `aws-sdk-go` (hashicorp/aws-sdk-go),
      a fork of the offical `awslabs` repo. We forked for stability while
      `awslabs` refactored the library, and will move back to the officially
      supported version in the next release.

IMPROVEMENTS:

  * **New config function: `format`** - Format a string using `sprintf`
      format. [GH-1096]
  * **New config function: `replace`** - Search and replace string values.
      Search can be a regular expression. See documentation for more
      info. [GH-1029]
  * **New config function: `split`** - Split a value based on a delimiter.
      This is useful for faking lists as parameters to modules.
  * **New resource: `digitalocean_ssh_key`** [GH-1074]
  * config: Expand `~` with homedir in `file()` paths [GH-1338]
  * core: The serial of the state is only updated if there is an actual
      change. This will lower the amount of state changing on things
      like refresh.
  * core: Autoload `terraform.tfvars.json` as well as `terraform.tfvars` [GH-1030]
  * core: `.tf` files that start with a period are now ignored. [GH-1227]
  * command/remote-config: After enabling remote state, a `pull` is
      automatically done initially.
  * providers/google: Add `size` option to disk blocks for instances. [GH-1284]
  * providers/aws: Improve support for tagging resources.
  * providers/aws: Add a short syntax for Route 53 Record names, e.g.
      `www` instead of `www.example.com`.
  * providers/aws: Improve dependency violation error handling, when deleting
      Internet Gateways or Auto Scaling groups [GH-1325].
  * provider/aws: Add non-destructive updates to AWS RDS. You can now upgrade
      `egine_version`, `parameter_group_name`, and `multi_az` without forcing
      a new database to be created.[GH-1341]
  * providers/aws: Full support for block device mappings on instances and
      launch configurations [GH-1045, GH-1364]
  * provisioners/remote-exec: SSH agent support. [GH-1208]

BUG FIXES:

  * core: module outputs can be used as inputs to other modules [GH-822]
  * core: Self-referencing splat variables are no longer allowed in
      provisioners. [GH-795][GH-868]
  * core: Validate that `depends_on` doesn't contain interpolations. [GH-1015]
  * core: Module inputs can be non-strings. [GH-819]
  * core: Fix invalid plan that resulted in "diffs don't match" error when
      a computed attribute was used as part of a set parameter. [GH-1073]
  * core: Fix edge case where state containing both "resource" and
      "resource.0" would ignore the latter completely. [GH-1086]
  * core: Modules with a source of a relative file path moving up
      directories work properly, i.e. "../a" [GH-1232]
  * providers/aws: manually deleted VPC removes it from the state
  * providers/aws: `source_dest_check` regression fixed (now works). [GH-1020]
  * providers/aws: Longer wait times for DB instances.
  * providers/aws: Longer wait times for route53 records (30 mins). [GH-1164]
  * providers/aws: Fix support for TXT records in Route 53. [GH-1213]
  * providers/aws: Fix support for wildcard records in Route 53. [GH-1222]
  * providers/aws: Fix issue with ignoring the 'self' attribute of a
      Security Group rule. [GH-1223]
  * providers/aws: Fix issue with `sql_mode` in RDS parameter group always
      causing an update. [GH-1225]
  * providers/aws: Fix dependency violation with subnets and security groups
      [GH-1252]
  * providers/aws: Fix issue with refreshing `db_subnet_groups` causing an error
      instead of updating state [GH-1254]
  * providers/aws: Prevent empty string to be used as default
      `health_check_type` [GH-1052]
  * providers/aws: Add tags on AWS IG creation, not just on update [GH-1176]
  * providers/digitalocean: Waits until droplet is ready to be destroyed [GH-1057]
  * providers/digitalocean: More lenient about 404's while waiting [GH-1062]
  * providers/digitalocean: FQDN for domain records in CNAME, MX, NS, etc.
      Also fixes invalid updates in plans. [GH-863]
  * providers/google: Network data in state was not being stored. [GH-1095]
  * providers/heroku: Fix panic when config vars block was empty. [GH-1211]

PLUGIN CHANGES:

  * New `helper/schema` fields for resources: `Deprecated` and `Removed` allow
      plugins to generate warning or error messages when a given attribute is used.

## 0.3.7 (February 19, 2015)

IMPROVEMENTS:

  * **New resources: `google_compute_forwarding_rule`, `google_compute_http_health_check`,
      and `google_compute_target_pool`** - Together these provide network-level
      load balancing. [GH-588]
  * **New resource: `aws_main_route_table_association`** - Manage the main routing table
      of a VPC. [GH-918]
  * **New resource: `aws_vpc_peering_connection`** [GH-963]
  * core: Formalized the syntax of interpolations and documented it
      very heavily.
  * core: Strings in interpolations can now contain further interpolations,
      e.g.: `foo ${bar("${baz}")}`.
  * provider/aws: Internet gateway supports tags [GH-720]
  * provider/aws: Support the more standard environmental variable names
      for access key and secret keys. [GH-851]
  * provider/aws: The `aws_db_instance` resource no longer requires both
      `final_snapshot_identifier` and `skip_final_snapshot`; the presence or
      absence of the former now implies the latter. [GH-874]
  * provider/aws: Avoid unnecessary update of `aws_subnet` when
      `map_public_ip_on_launch` is not specified in config. [GH-898]
  * provider/aws: Add `apply_method` to `aws_db_parameter_group` [GH-897]
  * provider/aws: Add `storage_type` to `aws_db_instance` [GH-896]
  * provider/aws: ELB can update listeners without requiring new. [GH-721]
  * provider/aws: Security group support egress rules. [GH-856]
  * provider/aws: Route table supports VPC peering connection on route. [GH-963]
  * provider/aws: Add `root_block_device` to `aws_db_instance` [GH-998]
  * provider/google: Remove "client secrets file", as it's no longer necessary
      for API authentication [GH-884].
  * provider/google: Expose `self_link` on `google_compute_instance` [GH-906]

BUG FIXES:

  * core: Fixing use of remote state with plan files. [GH-741]
  * core: Fix a panic case when certain invalid types were used in
      the configuration. [GH-691]
  * core: Escape characters `\"`, `\n`, and `\\` now work in interpolations.
  * core: Fix crash that could occur when there are exactly zero providers
      installed on a system. [GH-786]
  * core: JSON TF configurations can configure provisioners. [GH-807]
  * core: Sort `depends_on` in state to prevent unnecessary file changes. [GH-928]
  * core: State containing the zero value won't cause a diff with the
      lack of a value. [GH-952]
  * core: If a set type becomes empty, the state will be properly updated
      to remove it. [GH-952]
  * core: Bare "splat" variables are not allowed in provisioners. [GH-636]
  * core: Invalid configuration keys to sub-resources are now errors. [GH-740]
  * command/apply: Won't try to initialize modules in some cases when
      no arguments are given. [GH-780]
  * command/apply: Fix regression where user variables weren't asked [GH-736]
  * helper/hashcode: Update `hash.String()` to always return a positive index.
      Fixes issue where specific strings would convert to a negative index
      and be omitted when creating Route53 records. [GH-967]
  * provider/aws: Automatically suffix the Route53 zone name on record names. [GH-312]
  * provider/aws: Instance should ignore root EBS devices. [GH-877]
  * provider/aws: Fix `aws_db_instance` to not recreate each time. [GH-874]
  * provider/aws: ASG termination policies are synced with remote state. [GH-923]
  * provider/aws: ASG launch configuration setting can now be updated in-place. [GH-904]
  * provider/aws: No read error when subnet is manually deleted. [GH-889]
  * provider/aws: Tags with empty values (empty string) are properly
      managed. [GH-968]
  * provider/aws: Fix case where route table would delete its routes
      on an unrelated change. [GH-990]
  * provider/google: Fix bug preventing instances with metadata from being
      created [GH-884].

PLUGIN CHANGES:

  * New `helper/schema` type: `TypeFloat` [GH-594]
  * New `helper/schema` field for resources: `Exists` must point to a function
      to check for the existence of a resource. This is used to properly
      handle the case where the resource was manually deleted. [GH-766]
  * There is a semantic change in `GetOk` where it will return `true` if
      there is any value in the diff that is _non-zero_. Before, it would
      return true only if there was a value in the diff.

## 0.3.6 (January 6, 2015)

FEATURES:

  * **New provider: `cloudstack`**

IMPROVEMENTS:

  * **New resource: `aws_key_pair`** - Import a public key into AWS. [GH-695]
  * **New resource: `heroku_cert`** - Manage Heroku app certs.
  * provider/aws: Support `eu-central-1`, `cn-north-1`, and GovCloud. [GH-525]
  * provider/aws: `route_table` can have tags. [GH-648]
  * provider/google: Support Ubuntu images. [GH-724]
  * provider/google: Support for service accounts. [GH-725]

BUG FIXES:

  * core: temporary/hidden files that look like Terraform configurations
      are no longer loaded. [GH-548]
  * core: Set types in resources now result in deterministic states,
      resulting in cleaner plans. [GH-663]
  * core: fix issue where "diff was not the same" would come up with
      diffing lists. [GH-661]
  * core: fix crash where module inputs weren't strings, and add more
      validation around invalid types here. [GH-624]
  * core: fix error when using a computed module output as an input to
      another module. [GH-659]
  * core: map overrides in "terraform.tfvars" no longer result in a syntax
      error. [GH-647]
  * core: Colon character works in interpolation [GH-700]
  * provider/aws: Fix crash case when internet gateway is not attached
      to any VPC. [GH-664]
  * provider/aws: `vpc_id` is no longer required. [GH-667]
  * provider/aws: `availability_zones` on ELB will contain more than one
      AZ if it is set as such. [GH-682]
  * provider/aws: More fields are marked as "computed" properly, resulting
      in more accurate diffs for AWS instances. [GH-712]
  * provider/aws: Fix panic case by using the wrong type when setting
      volume size for AWS instances. [GH-712]
  * provider/aws: route table ignores routes with 'EnableVgwRoutePropagation'
      origin since those come from gateways. [GH-722]
  * provider/aws: Default network ACL ID and default security group ID
      support for `aws_vpc`. [GH-704]
  * provider/aws: Tags are not marked as computed. This introduces another
      issue with not detecting external tags, but this will be fixed in
      the future. [GH-730]

## 0.3.5 (December 9, 2014)

FEATURES:

 * **Remote State**: State files can now be stored remotely via HTTP,
     Consul, or HashiCorp's Atlas.
 * **New Provider: `atlas`**: Retrieve artifacts for deployment from
     HashiCorp's Atlas service.
 * New `element()` function to index into arrays

IMPROVEMENTS:

  * provider/aws: Support tenancy for aws\_instance
  * provider/aws: Support block devices for aws\_instance
  * provider/aws: Support virtual\_name on block device
  * provider/aws: Improve RDS reliability (more grace time)
  * provider/aws: Added aws\_db\_parameter\_group resource
  * provider/aws: Added tag support to aws\_subnet
  * provider/aws: Routes in RouteTable are optional
  * provider/aws: associate\_public\_ip\_address on aws\_launch\_configuration
  * provider/aws: Added aws\_network\_acl
  * provider/aws: Ingress rules in security groups are optional
  * provider/aws: Support termination policy for ASG
  * provider/digitalocean: Improved droplet size compatibility

BUG FIXES:

  * core: Fixed issue causing double delete. [GH-555]
  * core: Fixed issue with create-before-destroy not being respected in
      some circumstances.
  * core: Fixing issue with count expansion with non-homogenous instance
      plans.
  * core: Fix issue with referencing resource variables from resources
      that don't exist yet within resources that do exist, or modules.
  * core: Fixing depedency handling for modules
  * core: Fixing output handling [GH-474]
  * core: Fixing count interpolation in modules
  * core: Fixing multi-var without module state
  * core: Fixing HCL variable declaration
  * core: Fixing resource interpolation for without state
  * core: Fixing handling of computed maps
  * command/init: Fixing recursion issue [GH-518]
  * command: Validate config before requesting input [GH-602]
  * build: Fixing GOPATHs with spaces

MISC:

  * provider/aws: Upgraded to helper.Schema
  * provider/heroku: Upgraded to helper.Schema
  * provider/mailgun: Upgraded to helper.Schema
  * provider/dnsimple: Upgraded to helper.Schema
  * provider/cloudflare: Upgraded to helper.Schema
  * provider/digitalocean: Upgraded to helper.Schema
  * provider/google: Upgraded to helper.Schema

## 0.3.1 (October 21, 2014)

IMPROVEMENTS:

  * providers/aws: Support tags for security groups.
  * providers/google: Add "external\_address" to network attributes [GH-454]
  * providers/google: External address is used as default connection host. [GH-454]
  * providers/heroku: Support `locked` and `personal` booleans on organization
      settings. [GH-406]

BUG FIXES:

  * core: Remove panic case when applying with a plan that generates no
      new state. [GH-403]
  * core: Fix a hang that can occur with enough resources. [GH-410]
  * core: Config validation will not error if the field is being
      computed so the value is still unknown.
  * core: If a resource fails to create and has provisioners, it is
      marked as tainted. [GH-434]
  * core: Set types are validated to be sets. [GH-413]
  * core: String types are validated properly. [GH-460]
  * core: Fix crash case when destroying with tainted resources. [GH-412]
  * core: Don't execute provisioners in some cases on destroy.
  * core: Inherited provider configurations will be properly interpolated. [GH-418]
  * core: Refresh works properly if there are outputs that depend on resources
      that aren't yet created. [GH-483]
  * providers/aws: Refresh of launch configs and autoscale groups load
      the correct data and don't incorrectly recreate themselves. [GH-425]
  * providers/aws: Fix case where ELB would incorrectly plan to modify
      listeners (with the same data) in some cases.
  * providers/aws: Retry destroying internet gateway for some amount of time
      if there is a dependency violation since it is probably just eventual
      consistency (public facing resources being destroyed). [GH-447]
  * providers/aws: Retry deleting security groups for some amount of time
      if there is a dependency violation since it is probably just eventual
      consistency. [GH-436]
  * providers/aws: Retry deleting subnet for some amount of time if there is a
      dependency violation since probably asynchronous destroy events take
      place still. [GH-449]
  * providers/aws: Drain autoscale groups before deleting. [GH-435]
  * providers/aws: Fix crash case if launch config is manually deleted. [GH-421]
  * providers/aws: Disassociate EIP before destroying.
  * providers/aws: ELB treats subnets as a set.
  * providers/aws: Fix case where in a destroy/create tags weren't reapplied. [GH-464]
  * providers/aws: Fix incorrect/erroneous apply cases around security group
      rules. [GH-457]
  * providers/consul: Fix regression where `key` param changed to `keys. [GH-475]

## 0.3.0 (October 14, 2014)

FEATURES:

  * **Modules**: Configuration can now be modularized. Modules can live on
    GitHub, BitBucket, Git/Hg repos, HTTP URLs, and file paths. Terraform
    automatically downloads/updates modules for you on request.
  * **New Command: `init`**. This command initializes a Terraform configuration
    from an existing Terraform module (also new in 0.3).
  * **New Command: `destroy`**. This command destroys infrastructure
    created with `apply`.
  * Terraform will ask for user input to fill in required variables and
    provider configurations if they aren't set.
  * `terraform apply MODULE` can be used as a shorthand to quickly build
    infrastructure from a module.
  * The state file format is now JSON rather than binary. This allows for
    easier machine and human read/write. Old binary state files will be
    automatically upgraded.
  * You can now specify `create_before_destroy` as an option for replacement
    so that new resources are created before the old ones are destroyed.
  * The `count` metaparameter can now contain interpolations (such as
    variables).
  * The current index for a resource with a `count` set can be interpolated
    using `${count.index}`.
  * Various paths can be interpolated with the `path.X` variables. For example,
    the path to the current module can be interpolated using `${path.module}`.

IMPROVEMENTS:

  * config: Trailing commas are now allowed for the final elements of lists.
  * core: Plugins are loaded from `~/.terraform.d/plugins` (Unix) or
    `%USERDATA%/terraform.d/plugins` (Windows).
  * command/show: With no arguments, it will show the default state. [GH-349]
  * helper/schema: Can now have default values. [GH-245]
  * providers/aws: Tag support for most resources.
  * providers/aws: New resource `db_subnet_group`. [GH-295]
  * providers/aws: Add `map_public_ip_on_launch` for subnets. [GH-285]
  * providers/aws: Add `iam_instance_profile` for instances. [GH-319]
  * providers/aws: Add `internal` option for ELBs. [GH-303]
  * providers/aws: Add `ssl_certificate_id` for ELB listeners. [GH-350]
  * providers/aws: Add `self` option for security groups for ingress
      rules with self as source. [GH-303]
  * providers/aws: Add `iam_instance_profile` option to
      `aws_launch_configuration`. [GH-371]
  * providers/aws: Non-destructive update of `desired_capacity` for
      autoscale groups.
  * providers/aws: Add `main_route_table_id` attribute to VPCs. [GH-193]
  * providers/consul: Support tokens. [GH-396]
  * providers/google: Support `target_tags` for firewalls. [GH-324]
  * providers/google: `google_compute_instance` supports `can_ip_forward` [GH-375]
  * providers/google: `google_compute_disk` supports `type` to support disks
      such as SSDs. [GH-351]
  * provisioners/local-exec: Output from command is shown in CLI output. [GH-311]
  * provisioners/remote-exec: Output from command is shown in CLI output. [GH-311]

BUG FIXES:

  * core: Providers are validated even without a `provider` block. [GH-284]
  * core: In the case of error, walk all non-dependent trees.
  * core: Plugin loading from CWD works properly.
  * core: Fix many edge cases surrounding the `count` meta-parameter.
  * core: Strings in the configuration can escape double-quotes with the
      standard `\"` syntax.
  * core: Error parsing CLI config will show properly. [GH-288]
  * core: More than one Ctrl-C will exit immediately.
  * providers/aws: autoscaling_group can be launched into a vpc [GH-259]
  * providers/aws: not an error when RDS instance is deleted manually. [GH-307]
  * providers/aws: Retry deleting subnet for some time while AWS eventually
      destroys dependencies. [GH-357]
  * providers/aws: More robust destroy for route53 records. [GH-342]
  * providers/aws: ELB generates much more correct plans without extranneous
      data.
  * providers/aws: ELB works properly with dynamically changing
      count of instances.
  * providers/aws: Terraform can handle ELBs deleted manually. [GH-304]
  * providers/aws: Report errors properly if RDS fails to delete. [GH-310]
  * providers/aws: Wait for launch configuration to exist after creation
      (AWS eventual consistency) [GH-302]

## 0.2.2 (September 9, 2014)

IMPROVEMENTS:

  * providers/amazon: Add `ebs_optimized` flag. [GH-260]
  * providers/digitalocean: Handle 404 on delete
  * providers/digitalocean: Add `user_data` argument for creating droplets
  * providers/google: Disks can be marked `auto_delete`. [GH-254]

BUG FIXES:

  * core: Fix certain syntax of configuration that could cause hang. [GH-261]
  * core: `-no-color` flag properly disables color. [GH-250]
  * core: "~" is expanded in `-var-file` flags. [GH-273]
  * core: Errors with tfvars are shown in console. [GH-269]
  * core: Interpolation function calls with more than two args parse. [GH-282]
  * providers/aws: Refreshing EIP from pre-0.2 state file won't error. [GH-258]
  * providers/aws: Creating EIP without an instance/network won't fail.
  * providers/aws: Refreshing EIP manually deleted works.
  * providers/aws: Retry EIP delete to allow AWS eventual consistency to
      detect it isn't attached. [GH-276]
  * providers/digitalocean: Handle situations when resource was destroyed
      manually. [GH-279]
  * providers/digitalocean: Fix a couple scenarios where the diff was
      incorrect (and therefore the execution as well).
  * providers/google: Attaching a disk source (not an image) works
      properly. [GH-254]

## 0.2.1 (August 31, 2014)

IMPROVEMENTS:

  * core: Plugins are automatically discovered in the executable directory
      or pwd if named properly. [GH-190]
  * providers/mailgun: domain records are now saved to state

BUG FIXES:

  * core: Configuration parses when identifier and '=' have no space. [GH-243]
  * core: `depends_on` with `count` generates the proper graph. [GH-244]
  * core: Depending on a computed variable of a list type generates a
      plan without failure. i.e. `${type.name.foos.0.bar}` where `foos`
      is computed. [GH-247]
  * providers/aws: Route53 destroys in parallel work properly. [GH-183]

## 0.2.0 (August 28, 2014)

BACKWARDS INCOMPATIBILITIES:

  * We've replaced the configuration language in use from a C library to
    a pure-Go reimplementation. In the process, we removed some features
    of the language since it was too flexible:
    * Semicolons are no longer valid at the end of lines
    * Keys cannot be double-quoted strings: `"foo" = "bar"` is no longer
      valid.
    * JSON style maps `{ "foo": "bar" }` are no longer valid outside of JSON.
      Maps must be in the format of `{ foo = "bar" }` (like other objects
      in the config)
  * Heroku apps now require (will not validate without) `region` and
    `name` due to an upstream API change. [GH-239]

FEATURES:

  * **New Provider: `google`**: Manage Google Compute instances, disks,
      firewalls, and more.
  * **New Provider: `mailgun`**: Manage mailgun domains.
  * **New Function: `concat`**: Concatenate multiple strings together.
    Example: `concat(var.region, "-", var.channel)`.

IMPROVEMENTS:

  * core: "~/.terraformrc" (Unix) or "%APPDATA%/terraform.rc" (Windows)
    can be used to configure custom providers and provisioners. [GH-192]
  * providers/aws: EIPs now expose `allocation_id` and `public_ip`
      attributes.
  * providers/aws: Security group rules can be updated without a
      destroy/create.
  * providers/aws: You can enable and disable dns settings for VPCs. [GH-172]
  * providers/aws: Can specify a private IP address for `aws_instance` [GH-217]

BUG FIXES:

  * core: Variables are validated to not contain interpolations. [GH-180]
  * core: Key files for provisioning can now contain `~` and will be expanded
      to the user's home directory. [GH-179]
  * core: The `file()` function can load files in sub-directories. [GH-213]
  * core: Fix issue where some JSON structures didn't map properly into
     Terraform structures. [GH-177]
  * core: Resources with only `file()` calls will interpolate. [GH-159]
  * core: Variables work in block names. [GH-234]
  * core: Plugins are searched for in the same directory as the executable
      before the PATH. [GH-157]
  * command/apply: "tfvars" file no longer interferes with plan apply. [GH-153]
  * providers/aws: Fix issues around failing to read EIPs. [GH-122]
  * providers/aws: Autoscaling groups now register and export load
    balancers. [GH-207]
  * providers/aws: Ingress results are treated as a set, so order doesn't
      matter anymore. [GH-87]
  * providers/aws: Instance security groups treated as a set [GH-194]
  * providers/aws: Retry Route53 requests if operation failed because another
      operation is in progress [GH-183]
  * providers/aws: Route53 records with multiple record values work. [GH-221]
  * providers/aws: Changing AMI doesn't result in errors anymore. [GH-196]
  * providers/heroku: If you delete the `config_vars` block, config vars
      are properly nuked.
  * providers/heroku: Domains and drains are deleted before the app.
  * providers/heroku: Moved from the client library bgentry/heroku-go to
      cyberdelia/heroku-go [GH-239].
  * providers/heroku: Plans without a specific plan name for
      heroku\_addon work. [GH-198]

PLUGIN CHANGES:

  * **New Package:** `helper/schema`. This introduces a high-level framework
    for easily writing new providers and resources. The Heroku provider has
    been converted to this as an example.

## 0.1.1 (August 5, 2014)

FEATURES:

  * providers/heroku: Now supports creating Heroku Drains [GH-97]

IMPROVEMENTS:

  * providers/aws: Launch configurations accept user data [GH-94]
  * providers/aws: Regions are now validated [GH-96]
  * providers/aws: ELB now supports health check configurations [GH-109]

BUG FIXES:

  * core: Default variable file "terraform.tfvars" is auto-loaded. [GH-59]
  * core: Multi-variables (`foo.*.bar`) work even when `count = 1`. [GH-115]
  * core: `file()` function can have string literal arg [GH-145]
  * providers/cloudflare: Include the proper bins so the cloudflare
      provider is compiled
  * providers/aws: Engine version for RDS now properly set [GH-118]
  * providers/aws: Security groups now depend on each other and
  * providers/aws: DB instances now wait for destroys, have proper
      dependencies and allow passing skip_final_snapshot
  * providers/aws: Add associate_public_ip_address as an attribute on
      the aws_instance resource [GH-85]
  * providers/aws: Fix cidr blocks being updated [GH-65, GH-85]
  * providers/aws: Description is now required for security groups
  * providers/digitalocean: Private IP addresses are now a separate
      attribute
  * provisioner/all: If an SSH key is given with a password, a better
      error message is shown. [GH-73]

## 0.1.0 (July 28, 2014)

  * Initial release


