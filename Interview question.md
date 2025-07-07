**Top 50 Terraform Interview Questions and Answers with Scenarios**

---

### **Basics & Core Concepts**

1. **What is Terraform and why is it used?**
   **Answer**: Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp that allows you to define, provision, and manage infrastructure across various cloud providers using a declarative configuration language called HCL (HashiCorp Configuration Language).

2. **What are the main components of Terraform?**
   **Answer**:

* **Providers**: Interface with cloud platforms like AWS, Azure.
* **Resources**: Basic building blocks (e.g., AWS EC2).
* **Modules**: Reusable containers for resources.
* **State**: Keeps track of the infrastructure.
* **Variables**: Parameters for configurations.

3. **Difference between Terraform and other IaC tools?**
   **Answer**: Unlike Ansible or Chef (which are procedural and configuration-focused), Terraform is declarative and focuses on infrastructure provisioning. Terraform is cloud-agnostic and supports immutable infrastructure.

4. **What is a provider in Terraform?**
   **Answer**: Providers are plugins used to interact with APIs of cloud platforms and other services (e.g., AWS, Azure, Google Cloud, GitHub).

5. **What is a Terraform module and why would you use it?**
   **Answer**: Modules are containers for multiple resources used together. They help in code reusability, organization, and DRY principles.

6. **What is the purpose of `terraform init`?**
   **Answer**: Initializes the working directory, downloads providers, and prepares the backend.

7. **Explain `terraform plan` and `terraform apply`.**
   **Answer**:

* `terraform plan`: Shows what Terraform will do.
* `terraform apply`: Applies the changes.

8. **What is a Terraform state file?**
   **Answer**: `.tfstate` holds the mapping of real infrastructure to Terraform configurations, enabling tracking and diffs.

9. **Benefits of using Terraform?**
   **Answer**: Automation, idempotency, version control, cross-cloud provisioning, and consistent infrastructure.

10. **Explain Terraform's declarative approach.**
    **Answer**: You define *what* you want (not how to do it), and Terraform figures out the steps to get there.

---

### **Intermediate Terraform Questions**

11. **What happens during `terraform refresh`?**
    **Answer**: It updates the Terraform state file to reflect the real-world infrastructure.

12. **What is `terraform taint`?**
    **Answer**: Marks a resource for destruction and recreation in the next apply.

13. **How to destroy a single resource in Terraform?**
    **Answer**: Use `terraform destroy -target=resource_type.resource_name`.

14. **Difference: `terraform destroy` vs `terraform apply -destroy`?**
    **Answer**: Both destroy resources, but `apply -destroy` first shows a plan, then destroys.

15. **What are `locals` in Terraform?**
    **Answer**: `locals` define local variables to simplify and reuse expressions.

16. **How do you import existing resources?**
    **Answer**: Use `terraform import resource_type.resource_name resource_id`.

17. **Difference between `count` and `for_each`?**
    **Answer**:

* `count` is numeric.
* `for_each` works with maps/sets for better resource control.

18. **What are data sources?**
    **Answer**: Allow Terraform to query existing infrastructure outside its management.

19. **What is a backend?**
    **Answer**: Defines where Terraform stores state (e.g., local, S3, Consul).

20. **Purpose of `terraform output`?**
    **Answer**: Displays output values defined in config. Useful for passing data.

---

### **Advanced Questions & Scenarios**

21. **Scenario**: Want to manage manually created EC2 instance.
    **Answer**: Use `terraform import` with the instance ID and match config code to imported state.

22. **Scenario**: Prevent resource recreation.
    **Answer**: Use `lifecycle` block with `prevent_destroy = true` or fix the config change causing drift.

23. **What is Terraform drift?**
    **Answer**: When the real infrastructure differs from Terraform state. Use `terraform plan` or `terraform refresh` to detect.

24. **Scenario**: State conflict in remote backend.
    **Answer**: Use state locking via S3 + DynamoDB to prevent simultaneous changes.

25. **Explain workspaces.**
    **Answer**: Isolates state per environment (e.g., dev, prod). Use `terraform workspace` commands.

26. **How to manage secrets?**
    **Answer**: Use environment variables, Vault, SSM Parameter Store, or secret backends. Avoid hardcoding.

27. **Lifecycle rules?**
    **Answer**: Used to control resource behavior. Examples: `create_before_destroy`, `prevent_destroy`, `ignore_changes`.

28. **Prevent deletion of critical resource?**
    **Answer**: Use `prevent_destroy` in the `lifecycle` block.

29. **Code reuse across environments?**
    **Answer**: Use modules and pass different variables using `.tfvars` or workspace-specific files.

30. **Safe update of shared module?**
    **Answer**: Use versioning in modules and test changes in dev/staging before production.

---

### **Modules & Code Management**

31. **Root vs child module?**
    **Answer**: Root is main directory invoked via `terraform`. Child modules are included using `module` blocks.

32. **Pass variables to module?**
    **Answer**:

```hcl
module "vpc" {
  source = "./vpc"
  cidr_block = var.cidr_block
}
```

33. **Using Terraform Registry?**
    **Answer**: Reference as:

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "3.14.0"
}
```

34. **Best practices for modules?**
    **Answer**: Small, reusable, documented, input/output defined, version controlled.

35. **Reusable VPC module for 3 projects?**
    **Answer**: Create a shared `vpc` module and use it with different variables in three directories (dev, staging, prod).

---

### **Terraform State Management**

36. **Challenges with local state?**
    **Answer**: No locking, no collaboration, risk of loss, hard to manage.

37. **Migrate local state to remote?**
    **Answer**:

```sh
terraform init -migrate-state
```

Use backend config to point to S3 or other.

38. **What is state locking?**
    **Answer**: Prevents multiple users from changing the state simultaneously. Enabled with remote backend + DynamoDB.

39. **Deleted .tfstate file recovery?**
    **Answer**: Use backup `.tfstate.backup` file or recover from remote backend versioning (e.g., S3).

40. **What is .tfstate.backup?**
    **Answer**: Auto-generated backup of the last good state before the latest apply.

---

### **Real Projects & CI/CD**

41. **How to ensure review before apply?**
    **Answer**: Use Git + PR workflows, `terraform plan` in CI pipelines, and approval gates.

42. **CI/CD tools for Terraform?**
    **Answer**: Jenkins, GitLab CI, GitHub Actions, CircleCI, Atlantis.

43. **Scenario: `terraform apply` failed midway.**
    **Answer**: Check logs, fix issues, and re-run. Terraform resumes from last known state.

44. **Deploy in multiple regions?**
    **Answer**: Use provider aliasing:

```hcl
provider "aws" {
  region = "us-east-1"
  alias  = "use1"
}
```

45. **Multi-cloud deployment?**
    **Answer**: Use multiple providers (e.g., aws, azurerm) with appropriate resources and credentials.

---

### **Best Practices & Security**

46. **Terraform security best practices?**
    **Answer**:

* Store state securely (S3 + encryption).
* Don’t hardcode secrets.
* Use IAM roles and least privilege.
* Validate configs before apply.

47. **Manage sensitive data in tfvars?**
    **Answer**: Don’t commit tfvars to VCS. Use `.gitignore`, store in secure systems.

48. **Version control for modules and Terraform?**
    **Answer**: Pin module versions, use Terraform version constraints.

49. **Why use `terraform validate` and `terraform fmt`?**
    **Answer**: Ensures syntactic correctness and formatting consistency before committing code.

50. **Difference: `var.file` vs `terraform.tfvars`?**
    **Answer**:

* `var.file`: Explicit variable files.
* `terraform.tfvars`: Automatically loaded by Terraform.

---

Let me know if you'd like this exported as a PDF or formatted differently for practice or documentation.
