# Hashicorp Terraform Associate

Notes on the [course][1] from Udemy.

## General

- Use `terraform show` to get a full overview of the state, or `terraform state list` to only list resources
- Use `terraform providers` to get a list of required providers
- Use data blocks to fetch data from provider resources
- Use locals to define variables for expressions
- Provisioners (local-exec, remote-exec, file) are considered a last resort, use configuration management tools like Ansible instead
- Use `terraform taint <RESOURCE_NAME>` to mark a resource for recreation without changing source code
- Use `terraform import <RESOURCE_NAME> <RESOURCE_ID>` to import existing resources
- Workspaces can be used to manage different environments, but as state is stored in a single state file, they should be used with caution. Consider using dedicated state files for each environment instead. 
- Set the environment variable `TF_LOG` to *DEBUG* or *TRACE* to get more detailed logs. Set `TF_LOG_PATH` to a file to save logs to a file.
- `terraform validate` supports a `--json` flag to provide JSON output to be used in CI
- Use `terraform plan -refresh-only` to conduct drift detection
- Local state is stored in a file named `terraform.tfstate` by default
- AWS, Azure and GCP provide backends which support locking and state versioning
- Terraform Cloud is an enhanced backend with stores state and run operations remotely
- We can migrate state to a new backend using `terraform init -migrate-state`
- Use partial backend configuration files to work around the lack of support for variables in backend configuration
- Consider pulling secrets from third party solutions like AWS SecretsManager, Azure KeyVault or 1Password
- Use *dynamic* blocks with to avoid repeating the same block with different values
- Use *lifecyle* blocks to change resource lifecycles, i.e. preventing a resource from being destroyed, or creating new resources before destroying old ones
- Check out [Terraform built-in functions][2]
- Read the [Terraform style guide][3]
- Use `terraform graph` and [graphviz][4] to visualize the dependency graph

## Exam

- Take note of the order of precedence of variables (`-var` flags > `terraform.tfvars` > environment variables > default values)
- Check if TLS provider is relevant for the exam


[1]: https://www.udemy.com/course/terraform-hands-on-labs
[2]: https://developer.hashicorp.com/terraform/language/functions
[3]: https://developer.hashicorp.com/terraform/language/style
[4]: https://graphviz.org
