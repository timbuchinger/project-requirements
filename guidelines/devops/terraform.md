# Terraform Guidelines

## Project Structure
```
terraform/
├── environments/           # Environment-specific configurations
│   ├── dev/
│   ├── staging/
│   └── prod/
├── modules/               # Reusable module definitions
│   ├── networking/
│   ├── compute/
│   └── storage/
└── shared/               # Shared configurations
    ├── backends.tf
    └── versions.tf

module/
├── main.tf               # Main resource definitions
├── variables.tf          # Input variables
├── outputs.tf           # Output values
└── README.md           # Module documentation
```

## Core Principles
- Infrastructure as Code (IaC)
- Modular design
- State management
- Resource graph awareness
- Immutable infrastructure

## Code Organization
- One resource per file for large modules
- Consistent naming conventions
- Clear variable scoping
- Output standardization
- Resource tagging strategy

## Quality Standards
- Format check with `terraform fmt`
- Validate with `terraform validate`
- Lint with `tflint`
- Security scan with `tfsec`
- Cost estimation before apply

## Project Conventions
```hcl
# Example module structure
variable "environment" {
  type        = string
  description = "Environment name (dev/staging/prod)"
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

locals {
  common_tags = {
    Environment = var.environment
    ManagedBy   = "Terraform"
    Project     = var.project_name
  }
}

module "networking" {
  source = "../modules/networking"

  vpc_cidr          = var.vpc_cidr
  environment       = var.environment
  availability_zones = data.aws_availability_zones.available.names

  tags = merge(local.common_tags, var.additional_tags)
}
```

## State Management
- Remote state storage
- State locking mechanism
- Workspace usage
- State backup strategy
- Import existing resources

## Security Standards
- Encryption at rest
- Fine-grained IAM roles
- Secrets management
- Network security groups
- Audit logging

## Testing Strategy
- Terratest for integration tests
- Unit tests with built-in testing framework
- Policy compliance checks
- Plan verification
- Post-apply validation

## Backend Configuration
```hcl
terraform {
  backend "s3" {
    bucket         = "terraform-state"
    key            = "environment/project.tfstate"
    region         = "us-west-2"
    encrypt        = true
    dynamodb_table = "terraform-lock"
  }

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}
```

## Error Handling
- Clear error messages
- Graceful failure handling
- Recovery procedures
- Rollback strategies
- State recovery

## Performance Guidelines
- Parallel resource creation
- Dependency optimization
- Resource targeting
- Data source caching
- Plan/Apply optimization

## Module Design
```hcl
# Example reusable module
module "web_app" {
  source = "./modules/web_app"

  # Required parameters
  app_name    = "my-application"
  environment = var.environment

  # Optional parameters with defaults
  instance_type = "t3.micro"
  min_size     = 2
  max_size     = 4

  # Security
  vpc_id            = module.vpc.vpc_id
  private_subnets   = module.vpc.private_subnets
  allowed_cidr_blocks = ["10.0.0.0/8"]

  # Tags
  tags = local.common_tags
}
