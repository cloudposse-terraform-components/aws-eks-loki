---
tags:
  - component/eks/loki
  - layer/grafana
  - provider/aws
  - provider/helm
---

# Component: `eks-loki`

Grafana Loki is a set of resources that can be combined into a fully featured logging stack. Unlike other logging
systems, Loki is built around the idea of only indexing metadata about your logs: labels (just like Prometheus labels).
Log data itself is then compressed and stored in chunks in object stores such as S3 or GCS, or even locally on a
filesystem.

This component deploys the [grafana/loki](https://github.com/grafana/loki/tree/main/production/helm/loki) helm chart.

## Usage

**Stack Level**: Regional

Here's an example snippet for how to use this component.

```yaml
components:
  terraform:
    eks/loki:
      vars:
        enabled: true
        name: loki
        alb_controller_ingress_group_component_name: eks/alb-controller-ingress-group/internal
```

> [!IMPORTANT]
>
> We recommend using an internal ALB for logging services. You must connect to the private network to access the Loki
> endpoint.

<!-- prettier-ignore-start -->
<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0.0 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 4.0, < 6.0.0 |
| <a name="requirement_helm"></a> [helm](#requirement\_helm) | >= 2.0.0, < 3.0.0 |
| <a name="requirement_kubernetes"></a> [kubernetes](#requirement\_kubernetes) | >= 2.7.1, != 2.21.0 |
| <a name="requirement_random"></a> [random](#requirement\_random) | >= 2.3 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | >= 4.0, < 6.0.0 |
| <a name="provider_random"></a> [random](#provider\_random) | >= 2.3 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_alb_controller_ingress_group"></a> [alb\_controller\_ingress\_group](#module\_alb\_controller\_ingress\_group) | cloudposse/stack-config/yaml//modules/remote-state | 1.8.0 |
| <a name="module_basic_auth_ssm_parameters"></a> [basic\_auth\_ssm\_parameters](#module\_basic\_auth\_ssm\_parameters) | cloudposse/ssm-parameter-store/aws | 0.13.0 |
| <a name="module_dns_gbl_delegated"></a> [dns\_gbl\_delegated](#module\_dns\_gbl\_delegated) | cloudposse/stack-config/yaml//modules/remote-state | 1.8.0 |
| <a name="module_eks"></a> [eks](#module\_eks) | cloudposse/stack-config/yaml//modules/remote-state | 1.8.0 |
| <a name="module_iam_roles"></a> [iam\_roles](#module\_iam\_roles) | ../../account-map/modules/iam-roles | n/a |
| <a name="module_loki"></a> [loki](#module\_loki) | cloudposse/helm-release/aws | 0.10.1 |
| <a name="module_loki_storage"></a> [loki\_storage](#module\_loki\_storage) | cloudposse/s3-bucket/aws | 4.10.0 |
| <a name="module_loki_tls_label"></a> [loki\_tls\_label](#module\_loki\_tls\_label) | cloudposse/label/null | 0.25.0 |
| <a name="module_this"></a> [this](#module\_this) | cloudposse/label/null | 0.25.0 |

## Resources

| Name | Type |
|------|------|
| [random_pet.basic_auth_username](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/pet) | resource |
| [random_string.basic_auth_password](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/string) | resource |
| [aws_eks_cluster_auth.eks](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/eks_cluster_auth) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_additional_schema_config"></a> [additional\_schema\_config](#input\_additional\_schema\_config) | A list of additional `configs` for the `schemaConfig` for the Loki chart. This list will be merged with the default schemaConfig.config defined by `var.default_schema_config` | <pre>list(object({<br/>    from         = string<br/>    object_store = string<br/>    schema       = string<br/>    index = object({<br/>      prefix = string<br/>      period = string<br/>    })<br/>  }))</pre> | `[]` | no |
| <a name="input_additional_tag_map"></a> [additional\_tag\_map](#input\_additional\_tag\_map) | Additional key-value pairs to add to each map in `tags_as_list_of_maps`. Not added to `tags` or `id`.<br/>This is for some rare cases where resources want additional configuration of tags<br/>and therefore take a list of maps with tag key, value, and additional configuration. | `map(string)` | `{}` | no |
| <a name="input_alb_controller_ingress_group_component_name"></a> [alb\_controller\_ingress\_group\_component\_name](#input\_alb\_controller\_ingress\_group\_component\_name) | The name of the eks/alb-controller-ingress-group component. This should be an internal facing ALB | `string` | `"eks/alb-controller-ingress-group"` | no |
| <a name="input_atomic"></a> [atomic](#input\_atomic) | If set, installation process purges chart on fail. The wait flag will be set automatically if atomic is used. | `bool` | `true` | no |
| <a name="input_attributes"></a> [attributes](#input\_attributes) | ID element. Additional attributes (e.g. `workers` or `cluster`) to add to `id`,<br/>in the order they appear in the list. New attributes are appended to the<br/>end of the list. The elements of the list are joined by the `delimiter`<br/>and treated as a single ID element. | `list(string)` | `[]` | no |
| <a name="input_basic_auth_enabled"></a> [basic\_auth\_enabled](#input\_basic\_auth\_enabled) | If `true`, enabled Basic Auth for the Ingress service. A user and password will be created and stored in AWS SSM. | `bool` | `true` | no |
| <a name="input_chart"></a> [chart](#input\_chart) | Chart name to be installed. The chart name can be local path, a URL to a chart, or the name of the chart if `repository` is specified. It is also possible to use the `<repository>/<chart>` format here if you are running Terraform on a system that the repository has been added to with `helm repo add` but this is not recommended. | `string` | `"loki"` | no |
| <a name="input_chart_description"></a> [chart\_description](#input\_chart\_description) | Set release description attribute (visible in the history). | `string` | `"Loki is a horizontally-scalable, highly-available, multi-tenant log aggregation system inspired by Prometheus."` | no |
| <a name="input_chart_repository"></a> [chart\_repository](#input\_chart\_repository) | Repository URL where to locate the requested chart. | `string` | `"https://grafana.github.io/helm-charts"` | no |
| <a name="input_chart_values"></a> [chart\_values](#input\_chart\_values) | Additional values to yamlencode as `helm_release` values. | `any` | `{}` | no |
| <a name="input_chart_version"></a> [chart\_version](#input\_chart\_version) | Specify the exact chart version to install. If this is not specified, the latest version is installed. | `string` | `null` | no |
| <a name="input_cleanup_on_fail"></a> [cleanup\_on\_fail](#input\_cleanup\_on\_fail) | Allow deletion of new resources created in this upgrade when upgrade fails. | `bool` | `true` | no |
| <a name="input_context"></a> [context](#input\_context) | Single object for setting entire context at once.<br/>See description of individual variables for details.<br/>Leave string and numeric variables as `null` to use default value.<br/>Individual variable settings (non-null) override settings in context object,<br/>except for attributes, tags, and additional\_tag\_map, which are merged. | `any` | <pre>{<br/>  "additional_tag_map": {},<br/>  "attributes": [],<br/>  "delimiter": null,<br/>  "descriptor_formats": {},<br/>  "enabled": true,<br/>  "environment": null,<br/>  "id_length_limit": null,<br/>  "label_key_case": null,<br/>  "label_order": [],<br/>  "label_value_case": null,<br/>  "labels_as_tags": [<br/>    "unset"<br/>  ],<br/>  "name": null,<br/>  "namespace": null,<br/>  "regex_replace_chars": null,<br/>  "stage": null,<br/>  "tags": {},<br/>  "tenant": null<br/>}</pre> | no |
| <a name="input_create_namespace"></a> [create\_namespace](#input\_create\_namespace) | Create the Kubernetes namespace if it does not yet exist | `bool` | `true` | no |
| <a name="input_default_schema_config"></a> [default\_schema\_config](#input\_default\_schema\_config) | A list of default `configs` for the `schemaConfig` for the Loki chart. For new installations, the default schema config doesn't change. See https://grafana.com/docs/loki/latest/operations/storage/schema/#new-loki-installs | <pre>list(object({<br/>    from         = string<br/>    object_store = string<br/>    schema       = string<br/>    index = object({<br/>      prefix = string<br/>      period = string<br/>    })<br/>  }))</pre> | <pre>[<br/>  {<br/>    "from": "2024-04-01",<br/>    "index": {<br/>      "period": "24h",<br/>      "prefix": "index_"<br/>    },<br/>    "object_store": "s3",<br/>    "schema": "v13",<br/>    "store": "tsdb"<br/>  }<br/>]</pre> | no |
| <a name="input_delimiter"></a> [delimiter](#input\_delimiter) | Delimiter to be used between ID elements.<br/>Defaults to `-` (hyphen). Set to `""` to use no delimiter at all. | `string` | `null` | no |
| <a name="input_descriptor_formats"></a> [descriptor\_formats](#input\_descriptor\_formats) | Describe additional descriptors to be output in the `descriptors` output map.<br/>Map of maps. Keys are names of descriptors. Values are maps of the form<br/>`{<br/>  format = string<br/>  labels = list(string)<br/>}`<br/>(Type is `any` so the map values can later be enhanced to provide additional options.)<br/>`format` is a Terraform format string to be passed to the `format()` function.<br/>`labels` is a list of labels, in order, to pass to `format()` function.<br/>Label values will be normalized before being passed to `format()` so they will be<br/>identical to how they appear in `id`.<br/>Default is `{}` (`descriptors` output will be empty). | `any` | `{}` | no |
| <a name="input_eks_component_name"></a> [eks\_component\_name](#input\_eks\_component\_name) | The name of the eks component | `string` | `"eks/cluster"` | no |
| <a name="input_enabled"></a> [enabled](#input\_enabled) | Set to false to prevent the module from creating any resources | `bool` | `null` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | ID element. Usually used for region e.g. 'uw2', 'us-west-2', OR role 'prod', 'staging', 'dev', 'UAT' | `string` | `null` | no |
| <a name="input_helm_manifest_experiment_enabled"></a> [helm\_manifest\_experiment\_enabled](#input\_helm\_manifest\_experiment\_enabled) | Enable storing of the rendered manifest for helm\_release so the full diff of what is changing can been seen in the plan | `bool` | `false` | no |
| <a name="input_id_length_limit"></a> [id\_length\_limit](#input\_id\_length\_limit) | Limit `id` to this many characters (minimum 6).<br/>Set to `0` for unlimited length.<br/>Set to `null` for keep the existing setting, which defaults to `0`.<br/>Does not affect `id_full`. | `number` | `null` | no |
| <a name="input_kube_data_auth_enabled"></a> [kube\_data\_auth\_enabled](#input\_kube\_data\_auth\_enabled) | If `true`, use an `aws_eks_cluster_auth` data source to authenticate to the EKS cluster.<br/>Disabled by `kubeconfig_file_enabled` or `kube_exec_auth_enabled`. | `bool` | `false` | no |
| <a name="input_kube_exec_auth_aws_profile"></a> [kube\_exec\_auth\_aws\_profile](#input\_kube\_exec\_auth\_aws\_profile) | The AWS config profile for `aws eks get-token` to use | `string` | `""` | no |
| <a name="input_kube_exec_auth_aws_profile_enabled"></a> [kube\_exec\_auth\_aws\_profile\_enabled](#input\_kube\_exec\_auth\_aws\_profile\_enabled) | If `true`, pass `kube_exec_auth_aws_profile` as the `profile` to `aws eks get-token` | `bool` | `false` | no |
| <a name="input_kube_exec_auth_enabled"></a> [kube\_exec\_auth\_enabled](#input\_kube\_exec\_auth\_enabled) | If `true`, use the Kubernetes provider `exec` feature to execute `aws eks get-token` to authenticate to the EKS cluster.<br/>Disabled by `kubeconfig_file_enabled`, overrides `kube_data_auth_enabled`. | `bool` | `true` | no |
| <a name="input_kube_exec_auth_role_arn"></a> [kube\_exec\_auth\_role\_arn](#input\_kube\_exec\_auth\_role\_arn) | The role ARN for `aws eks get-token` to use | `string` | `""` | no |
| <a name="input_kube_exec_auth_role_arn_enabled"></a> [kube\_exec\_auth\_role\_arn\_enabled](#input\_kube\_exec\_auth\_role\_arn\_enabled) | If `true`, pass `kube_exec_auth_role_arn` as the role ARN to `aws eks get-token` | `bool` | `true` | no |
| <a name="input_kubeconfig_context"></a> [kubeconfig\_context](#input\_kubeconfig\_context) | Context to choose from the Kubernetes kube config file | `string` | `""` | no |
| <a name="input_kubeconfig_exec_auth_api_version"></a> [kubeconfig\_exec\_auth\_api\_version](#input\_kubeconfig\_exec\_auth\_api\_version) | The Kubernetes API version of the credentials returned by the `exec` auth plugin | `string` | `"client.authentication.k8s.io/v1beta1"` | no |
| <a name="input_kubeconfig_file"></a> [kubeconfig\_file](#input\_kubeconfig\_file) | The Kubernetes provider `config_path` setting to use when `kubeconfig_file_enabled` is `true` | `string` | `""` | no |
| <a name="input_kubeconfig_file_enabled"></a> [kubeconfig\_file\_enabled](#input\_kubeconfig\_file\_enabled) | If `true`, configure the Kubernetes provider with `kubeconfig_file` and use that kubeconfig file for authenticating to the EKS cluster | `bool` | `false` | no |
| <a name="input_kubernetes_namespace"></a> [kubernetes\_namespace](#input\_kubernetes\_namespace) | Kubernetes namespace to install the release into | `string` | `"monitoring"` | no |
| <a name="input_label_key_case"></a> [label\_key\_case](#input\_label\_key\_case) | Controls the letter case of the `tags` keys (label names) for tags generated by this module.<br/>Does not affect keys of tags passed in via the `tags` input.<br/>Possible values: `lower`, `title`, `upper`.<br/>Default value: `title`. | `string` | `null` | no |
| <a name="input_label_order"></a> [label\_order](#input\_label\_order) | The order in which the labels (ID elements) appear in the `id`.<br/>Defaults to ["namespace", "environment", "stage", "name", "attributes"].<br/>You can omit any of the 6 labels ("tenant" is the 6th), but at least one must be present. | `list(string)` | `null` | no |
| <a name="input_label_value_case"></a> [label\_value\_case](#input\_label\_value\_case) | Controls the letter case of ID elements (labels) as included in `id`,<br/>set as tag values, and output by this module individually.<br/>Does not affect values of tags passed in via the `tags` input.<br/>Possible values: `lower`, `title`, `upper` and `none` (no transformation).<br/>Set this to `title` and set `delimiter` to `""` to yield Pascal Case IDs.<br/>Default value: `lower`. | `string` | `null` | no |
| <a name="input_labels_as_tags"></a> [labels\_as\_tags](#input\_labels\_as\_tags) | Set of labels (ID elements) to include as tags in the `tags` output.<br/>Default is to include all labels.<br/>Tags with empty values will not be included in the `tags` output.<br/>Set to `[]` to suppress all generated tags.<br/>**Notes:**<br/>  The value of the `name` tag, if included, will be the `id`, not the `name`.<br/>  Unlike other `null-label` inputs, the initial setting of `labels_as_tags` cannot be<br/>  changed in later chained modules. Attempts to change it will be silently ignored. | `set(string)` | <pre>[<br/>  "default"<br/>]</pre> | no |
| <a name="input_name"></a> [name](#input\_name) | ID element. Usually the component or solution name, e.g. 'app' or 'jenkins'.<br/>This is the only ID element not also included as a `tag`.<br/>The "name" tag is set to the full `id` string. There is no tag with the value of the `name` input. | `string` | `null` | no |
| <a name="input_namespace"></a> [namespace](#input\_namespace) | ID element. Usually an abbreviation of your organization name, e.g. 'eg' or 'cp', to help ensure generated IDs are globally unique | `string` | `null` | no |
| <a name="input_regex_replace_chars"></a> [regex\_replace\_chars](#input\_regex\_replace\_chars) | Terraform regular expression (regex) string.<br/>Characters matching the regex will be removed from the ID elements.<br/>If not set, `"/[^a-zA-Z0-9-]/"` is used to remove all characters other than hyphens, letters and digits. | `string` | `null` | no |
| <a name="input_region"></a> [region](#input\_region) | AWS Region | `string` | n/a | yes |
| <a name="input_ssm_path_template"></a> [ssm\_path\_template](#input\_ssm\_path\_template) | A string template to be used to create paths in AWS SSM to store basic auth credentials for this service | `string` | `"/%s/basic-auth/%s"` | no |
| <a name="input_stage"></a> [stage](#input\_stage) | ID element. Usually used to indicate role, e.g. 'prod', 'staging', 'source', 'build', 'test', 'deploy', 'release' | `string` | `null` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | Additional tags (e.g. `{'BusinessUnit': 'XYZ'}`).<br/>Neither the tag keys nor the tag values will be modified by this module. | `map(string)` | `{}` | no |
| <a name="input_tenant"></a> [tenant](#input\_tenant) | ID element \_(Rarely used, not included by default)\_. A customer identifier, indicating who this instance of a resource is for | `string` | `null` | no |
| <a name="input_timeout"></a> [timeout](#input\_timeout) | Time in seconds to wait for any individual kubernetes operation (like Jobs for hooks). Defaults to `300` seconds | `number` | `300` | no |
| <a name="input_verify"></a> [verify](#input\_verify) | Verify the package before installing it. Helm uses a provenance file to verify the integrity of the chart; this must be hosted alongside the chart | `bool` | `false` | no |
| <a name="input_wait"></a> [wait](#input\_wait) | Will wait until all resources are in a ready state before marking the release as successful. It will wait for as long as `timeout`. Defaults to `true`. | `bool` | `true` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_basic_auth_username"></a> [basic\_auth\_username](#output\_basic\_auth\_username) | If enabled, the username for basic auth |
| <a name="output_id"></a> [id](#output\_id) | The ID of this deployment |
| <a name="output_metadata"></a> [metadata](#output\_metadata) | Block status of the deployed release |
| <a name="output_ssm_path_basic_auth_password"></a> [ssm\_path\_basic\_auth\_password](#output\_ssm\_path\_basic\_auth\_password) | If enabled, the path in AWS SSM to find the password for basic auth |
| <a name="output_url"></a> [url](#output\_url) | The hostname used for this Loki deployment |
<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
<!-- prettier-ignore-end -->

## References

- [cloudposse/terraform-aws-components](https://github.com/cloudposse/terraform-aws-components/tree/main/modules/eks/loki) -
  Cloud Posse's upstream component

[<img src="https://cloudposse.com/logo-300x69.svg" height="32" align="right"/>](https://cpco.io/homepage?utm_source=github&utm_medium=readme&utm_campaign=cloudposse-terraform-components/aws-eks-loki&utm_content=)
