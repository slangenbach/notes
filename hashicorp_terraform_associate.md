# Hashicorp Terraform Associate

Notes on the [course][1] from Udemy.

## General

- Use `terraform show` to get a full overview of the state, or `terraform state list` to only list resources
- Use `terraform providers` to get a list of required providers
- Use data blocks to fetch data from provider resources
- TODO: Read about workspaces
- Use locals to define variables for expressions
- Terraform supports multi-line comments using `/* ... */`
- Provisioners (local-exec, remote-exec, file) are considered a last resort, use configuration management tools like Ansible instead

## Exam

- Take note of the order of precedence of variables
- Check if TLS provider is relevant for the exam

[1]: https://www.udemy.com/course/terraform-hands-on-labs