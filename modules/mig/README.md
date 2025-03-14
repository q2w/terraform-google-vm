# Managed Instance Group (MIG)

This module is used to create a [google_compute_region_instance_group_manager](https://www.terraform.io/docs/providers/google/r/compute_region_instance_group_manager),
and optionally, an autoscaler and healthchecks.

## Usage

See the [simple example](../../examples/mig/simple) for a usage example.

## Upgrading

The current version is 2.X. The following guides are available to assist with upgrades:

- [1.X -> 2.0](../../docs/upgrading_to_mig_v2.0.md)

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| autoscaler\_name | Autoscaler name. When variable is empty, name will be derived from var.hostname. | `string` | `""` | no |
| autoscaling\_cpu | Autoscaling, cpu utilization policy block as single element array. https://www.terraform.io/docs/providers/google/r/compute_autoscaler#cpu_utilization | <pre>list(object({<br>    target            = number<br>    predictive_method = string<br>  }))</pre> | `[]` | no |
| autoscaling\_enabled | Creates an autoscaler for the managed instance group | `string` | `"false"` | no |
| autoscaling\_lb | Autoscaling, load balancing utilization policy block as single element array. https://www.terraform.io/docs/providers/google/r/compute_autoscaler#load_balancing_utilization | `list(map(number))` | `[]` | no |
| autoscaling\_metric | Autoscaling, metric policy block as single element array. https://www.terraform.io/docs/providers/google/r/compute_autoscaler#metric | <pre>list(object({<br>    name   = string<br>    target = number<br>    type   = string<br>  }))</pre> | `[]` | no |
| autoscaling\_mode | Operating mode of the autoscaling policy. If omitted, the default value is ON. https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_autoscaler#mode | `string` | `null` | no |
| autoscaling\_scale\_in\_control | Autoscaling, scale-in control block. https://www.terraform.io/docs/providers/google/r/compute_autoscaler#scale_in_control | <pre>object({<br>    fixed_replicas   = number<br>    percent_replicas = number<br>    time_window_sec  = number<br>  })</pre> | <pre>{<br>  "fixed_replicas": null,<br>  "percent_replicas": null,<br>  "time_window_sec": null<br>}</pre> | no |
| cooldown\_period | The number of seconds that the autoscaler should wait before it starts collecting information from a new instance. | `number` | `60` | no |
| distribution\_policy\_target\_shape | MIG target distribution shape (EVEN, BALANCED, ANY, ANY\_SINGLE\_ZONE) | `string` | `null` | no |
| distribution\_policy\_zones | The distribution policy, i.e. which zone(s) should instances be create in. Default is all zones in given region. | `list(string)` | `[]` | no |
| health\_check | Health check to determine whether instances are responsive and able to do work | <pre>object({<br>    type                = string<br>    initial_delay_sec   = number<br>    check_interval_sec  = number<br>    healthy_threshold   = number<br>    timeout_sec         = number<br>    unhealthy_threshold = number<br>    response            = string<br>    proxy_header        = string<br>    port                = number<br>    request             = string<br>    request_path        = string<br>    host                = string<br>    enable_logging      = bool<br>  })</pre> | <pre>{<br>  "check_interval_sec": 30,<br>  "enable_logging": false,<br>  "healthy_threshold": 1,<br>  "host": "",<br>  "initial_delay_sec": 30,<br>  "port": 80,<br>  "proxy_header": "NONE",<br>  "request": "",<br>  "request_path": "/",<br>  "response": "",<br>  "timeout_sec": 10,<br>  "type": "",<br>  "unhealthy_threshold": 5<br>}</pre> | no |
| health\_check\_name | Health check name. When variable is empty, name will be derived from var.hostname. | `string` | `""` | no |
| hostname | Hostname prefix for instances | `string` | `"default"` | no |
| instance\_template | Instance template self\_link used to create compute instances | `string` | n/a | yes |
| labels | Labels, provided as a map | `map(string)` | `{}` | no |
| max\_replicas | The maximum number of instances that the autoscaler can scale up to. This is required when creating or updating an autoscaler. The maximum number of replicas should not be lower than minimal number of replicas. | `number` | `10` | no |
| mig\_name | Managed instance group name. When variable is empty, name will be derived from var.hostname. | `string` | `""` | no |
| mig\_timeouts | Times for creation, deleting and updating the MIG resources. Can be helpful when using wait\_for\_instances to allow a longer VM startup time. | <pre>object({<br>    create = string<br>    update = string<br>    delete = string<br>  })</pre> | <pre>{<br>  "create": "5m",<br>  "delete": "15m",<br>  "update": "5m"<br>}</pre> | no |
| min\_replicas | The minimum number of replicas that the autoscaler can scale down to. This cannot be less than 0. | `number` | `2` | no |
| named\_ports | Named name and named port. https://cloud.google.com/load-balancing/docs/backend-service#named_ports | <pre>list(object({<br>    name = string<br>    port = number<br>  }))</pre> | `[]` | no |
| project\_id | The Google Cloud project ID | `string` | n/a | yes |
| region | The Google Cloud region where the managed instance group resides. | `string` | n/a | yes |
| scaling\_schedules | Autoscaling, scaling schedule block. https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_autoscaler#scaling_schedules | <pre>list(object({<br>    disabled              = bool<br>    duration_sec          = number<br>    min_required_replicas = number<br>    name                  = string<br>    schedule              = string<br>    time_zone             = string<br>  }))</pre> | `[]` | no |
| stateful\_disks | Disks created on the instances that will be preserved on instance delete. https://cloud.google.com/compute/docs/instance-groups/configuring-stateful-disks-in-migs | <pre>list(object({<br>    device_name = string<br>    delete_rule = string<br>  }))</pre> | `[]` | no |
| stateful\_ips | Statful IPs created on the instances that will be preserved on instance delete. https://cloud.google.com/compute/docs/instance-groups/configuring-stateful-ip-addresses-in-migs | <pre>list(object({<br>    interface_name = string<br>    delete_rule    = string<br>    is_external    = bool<br>  }))</pre> | `[]` | no |
| target\_pools | The target load balancing pools to assign this group to. | `list(string)` | `[]` | no |
| target\_size | The target number of running instances for this managed instance group. This value should always be explicitly set unless this resource is attached to an autoscaler, in which case it should never be set. | `number` | `1` | no |
| update\_policy | The rolling update policy. https://www.terraform.io/docs/providers/google/r/compute_region_instance_group_manager#rolling_update_policy | <pre>list(object({<br>    max_surge_fixed                = optional(number)<br>    instance_redistribution_type   = optional(string)<br>    max_surge_percent              = optional(number)<br>    max_unavailable_fixed          = optional(number)<br>    max_unavailable_percent        = optional(number)<br>    min_ready_sec                  = optional(number)<br>    replacement_method             = optional(string)<br>    minimal_action                 = string<br>    type                           = string<br>    most_disruptive_allowed_action = optional(string)<br>  }))</pre> | `[]` | no |
| wait\_for\_instances | Whether to wait for all instances to be created/updated before returning. Note that if this is set to true and the operation does not succeed, Terraform will continue trying until it times out. | `string` | `"false"` | no |

## Outputs

| Name | Description |
|------|-------------|
| apphub\_workload\_uri | Workload URI in CAIS style to be used by Apphub. |
| health\_check\_self\_links | All self\_links of healthchecks created for the instance group. |
| instance\_group | Instance-group url of managed instance group |
| instance\_group\_manager | An instance of google\_compute\_region\_instance\_group\_manager of the instance group. |
| self\_link | Self-link of managed instance group |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
