🔹 What are Terraform Workspaces?
A Terraform Workspace is an isolated environment with its own Terraform state. It allows you to manage multiple instances of your infrastructure with the same configuration in the same backend.

🔹 Default Behavior
By default, Terraform has a single workspace called default.

When you use terraform apply, Terraform uses the state in the current workspace.

🔹 Why Use Workspaces?
To manage multiple environments (e.g., dev, staging, prod) using the same configuration.

Isolate the state file for each environment without duplicating code.

Useful when you don’t want to maintain separate folders or backends for each environment.

🔹 Workspace Commands
Command	Description
terraform workspace list	Lists all available workspaces
terraform workspace new <name>	Creates a new workspace
terraform workspace select <name>	Switches to a specific workspace
terraform workspace show	Displays the current workspace
terraform workspace delete <name>	Deletes a workspace (except default)

🔹 Workspace Path and State
In local backend, each workspace stores its state in .terraform/ directory.

In remote backend (e.g., S3), the state path changes based on workspace.

Example:

ruby
Copy
Edit
s3://my-bucket/env:/terraform.tfstate
🔹 Using Workspaces in Code
You can reference the current workspace using:

hcl
Copy
Edit
terraform.workspace
Example:

hcl
Copy
Edit
resource "aws_s3_bucket" "example" {
  bucket = "my-bucket-${terraform.workspace}"
}
✅ Real-Life Use Case
🏗 Scenario: Multi-Environment Infrastructure
You're deploying the same app to dev, staging, and prod. Using workspaces:

bash
Copy
Edit
terraform workspace new dev
terraform apply       # Deploys to dev

terraform workspace new prod
terraform apply       # Deploys to prod
Each environment will have its own state file and resource instances.

❓ Interview Questions and Answers
🔸 Q1: What is a Terraform Workspace?
A: A workspace is an isolated instance of Terraform state used to manage multiple environments using the same configuration. Each workspace maintains its own state file.

🔸 Q2: How do workspaces differ from modules?
A:

Workspaces isolate state for different environments.

Modules are reusable configuration blocks.
They solve different problems—workspaces manage environment separation, while modules manage code reusability.

🔸 Q3: How do you create and switch workspaces?
bash
Copy
Edit
terraform workspace new staging
terraform workspace select staging
🔸 Q4: How does Terraform handle state in different workspaces?
A: Each workspace has its own state file. When using remote backends like S3, the state file path includes the workspace name to isolate environments.

🔸 Q5: Can we use workspaces for production-grade environment management?
A: Workspaces are good for lightweight environment separation. For complex setups, using separate backends or Terraform Cloud is preferred for better security and isolation.

🔸 Q6: Can terraform.workspace be used inside a module?
A: Yes, terraform.workspace is a global variable and can be used inside any module or resource block to dynamically adjust resource names or values based on the workspace.

🔧 Real-Life Questions You May Face in Interviews
⚙️ Real-Life Q1:
"You have a Terraform configuration and want to deploy to both staging and production using the same code. How would you handle that?"

✅ Answer:

I’d use Terraform workspaces.

I’d run terraform workspace new staging and terraform workspace new prod.

I'd use terraform.workspace in resource names to dynamically differentiate resources.

This way, state files and deployments are isolated without code duplication.

⚙️ Real-Life Q2:
"Why might you avoid workspaces in a large-scale Terraform setup?"

✅ Answer:

Workspaces only isolate state, not configuration or secrets.

For complex production use cases, it's better to use:

Separate folders

Separate backends

CI/CD pipelines

Environment-specific configurations

⚠️ Workspace Best Practices
Avoid using workspaces for completely different infrastructures.

Use terraform.workspace for dynamic naming to prevent conflicts.

Use version control and automation (e.g., GitHub Actions or Jenkins) to handle workspace switching safely.

Always check current workspace with terraform workspace show before applying changes.
