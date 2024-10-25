<!-- BEGIN_TF_DOCS -->
# Spacelift Stack Module

This is a Terraform module that creates a Spacelift stack.
Not everything is implemented yet, but we are actively building this project out.
Please open a PR or an issue if you see missing functionality.

## Example

```hcl
module "ec2_worker_pool_stack" {
  source = "../../"

  name            = "worker-pool-stack"
  description     = "Stack to create a worker pool"
  repository_name = "spacelift"
  project_root    = "aws/ecs-worker-pool"

  auto_deploy = true

  additional_project_globs = [
    "modules/spacelift/worker-pool/**/*"
  ]

  environment_variables = {
    TF_VAR_worker_pool_config = {
      sensitive = true
      value = jsonencode({
        token       = spacelift_worker_pool.this.config
        private_key = base64encode(tls_private_key.this.private_key_pem)
      })
    }
  }

  space_id = spacelift_space.aws.id
  aws_integration = {
    enabled = true
    id      = spacelift_aws_integration.work.id
  }
}
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_additional_project_globs"></a> [additional\_project\_globs](#input\_additional\_project\_globs) | Additional project globs to add to the stack. | `list(string)` | `[]` | no |
| <a name="input_administrative"></a> [administrative](#input\_administrative) | Whether the stack is administrative or not. | `bool` | `false` | no |
| <a name="input_allow_promotion"></a> [allow\_promotion](#input\_allow\_promotion) | Whether to allow promotion of the stack to the next environment. | `bool` | `true` | no |
| <a name="input_auto_deploy"></a> [auto\_deploy](#input\_auto\_deploy) | Whether to auto deploy the stack. | `bool` | `false` | no |
| <a name="input_aws_integration"></a> [aws\_integration](#input\_aws\_integration) | Spacelift AWS integration configuration | <pre>object({<br/>    enabled = bool<br/>    id      = optional(string)<br/>  })</pre> | <pre>{<br/>  "enabled": false<br/>}</pre> | no |
| <a name="input_bitbucket_cloud_namespace"></a> [bitbucket\_cloud\_namespace](#input\_bitbucket\_cloud\_namespace) | The namespace of the Bitbucket Cloud account to use for the stack. Required if cloud\_integration is BITBUCKET. | `string` | `null` | no |
| <a name="input_cloud_integration"></a> [cloud\_integration](#input\_cloud\_integration) | The cloud integration to use for the stack. BITBUCKET or GITHUB. | `string` | `"GITHUB"` | no |
| <a name="input_cloudformation"></a> [cloudformation](#input\_cloudformation) | Cloudformation integration configuration | <pre>object({<br/>    stack_name          = string<br/>    entry_template_file = string<br/>    region              = string<br/>    template_bucket     = string<br/>  })</pre> | `null` | no |
| <a name="input_description"></a> [description](#input\_description) | REQUIRED A description to describe your Spacelift stack. | `string` | n/a | yes |
| <a name="input_environment_variables"></a> [environment\_variables](#input\_environment\_variables) | Environment variables to add to the context. | <pre>map(object({<br/>    value     = string<br/>    sensitive = optional(bool, false)<br/>  }))</pre> | `{}` | no |
| <a name="input_labels"></a> [labels](#input\_labels) | Labels to apply to the stack being created. | `list(string)` | `[]` | no |
| <a name="input_manage_state"></a> [manage\_state](#input\_manage\_state) | Should spacelift manage state files | `bool` | `true` | no |
| <a name="input_name"></a> [name](#input\_name) | REQUIRED The name of the Spacelift stack to create. | `string` | n/a | yes |
| <a name="input_policies"></a> [policies](#input\_policies) | Policies to add to the stack. | <pre>map(object({<br/>    file_path = string<br/>    type      = string<br/>  }))</pre> | `{}` | no |
| <a name="input_project_root"></a> [project\_root](#input\_project\_root) | The path to your project root in your repository to use as the root of the stack. Defaults to root of the repository. | `string` | `null` | no |
| <a name="input_repository_branch"></a> [repository\_branch](#input\_repository\_branch) | The name of the branch to use for the specified Git repository. | `string` | `"main"` | no |
| <a name="input_repository_name"></a> [repository\_name](#input\_repository\_name) | REQUIRED The name of the Git repository for the stack to use. | `string` | n/a | yes |
| <a name="input_runner_image"></a> [runner\_image](#input\_runner\_image) | The runner image to use for the stack. Defaults to the latest version. | `string` | `null` | no |
| <a name="input_space_id"></a> [space\_id](#input\_space\_id) | REQUIRED The ID of the space this stack will be in. | `string` | n/a | yes |
| <a name="input_terragrunt_config"></a> [terragrunt\_config](#input\_terragrunt\_config) | config for terragrunt in spacelift | <pre>object({<br/>    terraform_version    = string<br/>    terragrunt_version   = string<br/>    use_run_all          = optional(bool)<br/>    use_smart_sanitation = optional(bool)<br/>    tool                 = string<br/>  })</pre> | <pre>{<br/>  "terraform_version": null,<br/>  "terragrunt_version": null,<br/>  "tool": null<br/>}</pre> | no |
| <a name="input_tf_version"></a> [tf\_version](#input\_tf\_version) | The version of OpenTofu/Terraform for your stack to use. Defaults to latest. | `string` | `"1.7.1"` | no |
| <a name="input_tofu_workspace"></a> [tofu\_workspace](#input\_tofu\_workspace) | The workspace to use for the stack. | `string` | `null` | no |
| <a name="input_worker_pool_id"></a> [worker\_pool\_id](#input\_worker\_pool\_id) | The ID of the worker pool to use for Spacelift stack runs. Defaults to public worker pool. | `string` | `null` | no |
| <a name="input_workflow_tool"></a> [workflow\_tool](#input\_workflow\_tool) | The terraform workflow tool to use | `string` | `"OPEN_TOFU"` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_id"></a> [id](#output\_id) | The ID of the stack |  
<!-- END_TF_DOCS -->