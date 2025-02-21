# Hashicorp Terraform Associate

Notes on the [course][1] from Udemy.

## General

- Use `terraform show` to get a full *overview* of the state, or `terraform state list` to only list resources
- Use `terraform providers` to get a list of required providers
- Providers support aliases to manage multiple configurations of the same provider
- Use data blocks to fetch data from provider resources
- Use locals to define variables for expressions
- Provisioners (local-exec, remote-exec, file) are considered a last resort, use configuration management tools like Ansible instead
- Use `terraform taint <RESOURCE_NAME>` to mark a resource for recreation without changing source code (note that this has been replaced by `terraform apply -replace=<RESOURCE_NAME>`)
- Use `terraform import <RESOURCE_NAME> <RESOURCE_ID>` to import existing resources
- Mind that, before importing, the resource must be defined in the configuration
- Workspaces can be used to manage different environments, as state is stored in separate state file. They should however be used with caution.
- Set the environment variable `TF_LOG` to *DEBUG* or *TRACE* to get more detailed logs. Set `TF_LOG_PATH` to a file to save logs to a file.
- `terraform validate` supports a `--json` flag to provide JSON output to be used in CI
- Use `terraform plan -refresh-only` to conduct drift detection
- Local state is stored in a file named `terraform.tfstate` by default
- AWS, Azure and GCP provide backends which support locking and state versioning
- We can migrate state to a new backend using `terraform init -migrate-state`
- Use partial backend configuration files to work around the lack of support for variables in backend configuration
- Consider pulling secrets from third party solutions like AWS SecretsManager, Azure KeyVault or 1Password
- Use *dynamic* blocks with to avoid repeating the same block with different values
- Use *lifecyle* blocks to change resource lifecycles, i.e. preventing a resource from being destroyed, or creating new resources before destroying old ones
- Check out [Terraform built-in functions][2]
- Read the [Terraform style guide][3]
- Use `terraform graph` and [graphviz][4] to visualize the dependency graph

## Terraform Cloud

- Terraform Cloud is an enhanced backend with stores state and run operations remotely. It also features fine-grained access control (via Sentinel), policy enforcement, web UI, integration with version control systems and a private module registry
- Terraform Cloud private registry can be used to limit access to approved modules only
- Variables can also be applied to just a single run within a workspace
- Variable sets allow us to reuse variables across workspaces
- Sentinel has three policy enforcement levels: advisory (allowed to fail), soft-mandatory (must be overwritten when failing) and hard-mandatory

## Exam

- Take note of the order of precedence of variables (`-var` flags > `terraform.tfvars` > `TF_VAR` environment variables > default values)
- Sentinel runs before configuration is applied, and can be used to enforce policies
- Terraform uses state to map resources to the real world
- Use `terraform init` and `terraform get` to download modules
- Use `terraform workspace create` to create new workspaces
- Use `terraform workspace select` to switch workspaces
- Use `terraform workspace show` to show the current workspace
- *github* is not a valid backend type
- Use `terraform force-unlock` to unlock a locked state
- Per default Terraform creates 10 resources in parallel
- Per default Terraform uses 2 spaces for indentation
- Use `terraform apply -replace=<RESOURCE_NAME>` to replace a single resource
- Terraform is a **immutable**, declarative language based on HCL and optionally JSON
- Secrets will always be stored unencrypted in the state file, no matter the provider
- Terraform is available on Windows, Linux and MacOS and FreeBSD :eyes:
- The local state for **workspaces** is stored in `terraform.tfstate.d/<WORKSPACE_NAME>`
- Inspection of cloud resources is not a feature of Terraform state
- In both Terraform OSS and HCL workspaces provide similar functionality
- While it's best practices to declare provider blocks, it's not requirement
- Providers can be configured using the `provider` block only
- Using `terraform console` locks the state file
- Use `terraform apply -refresh=false` to skip refreshing the state during apply
- Use `terraform output` to get the value of outputs variables
- Use `[*]` to get the value of all values of a resource

[1]: https://www.udemy.com/course/terraform-hands-on-labs
[2]: https://developer.hashicorp.com/terraform/language/functions
[3]: https://developer.hashicorp.com/terraform/language/style
[4]: https://graphviz.org
