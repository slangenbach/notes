# Hashicorp Terraform Associate

Notes on the [course][1] from Udemy.

## General

- Use `terraform show` to get a full overview of the state, or `terraform state list` to only list resources
- Use `terraform providers` to get a list of required providers
- Use data blocks to fetch data from provider resources
- Use locals to define variables for expressions
- Terraform supports multi-line comments using `/* ... */`
- Provisioners (local-exec, remote-exec, file) are considered a last resort, use configuration management tools like Ansible instead
- Use `terraform taint <RESOURCE_NAME>` to mark a resource for recreation without changing source code
- Use `terraform import <RESOURCE_NAME> <RESOURCE_ID>` to import existing resources
- Workspaces can be used to manage different environments, but as state is stored in a single state file, they should be used with caution. Consider using dedicated state files for each environment instead. 
- Set the environment variable `TF_LOG` to *DEBUG* or *TRACE* to get more detailed logs. Set `TF_LOG_PATH` to a file to save logs to a file.

## Exam

- Take note of the order of precedence of variables
- Check if TLS provider is relevant for the exam

[1]: https://www.udemy.com/course/terraform-hands-on-labs