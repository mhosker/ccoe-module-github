# CCoE Module - GitHub
This Terraform module allows you to manage your GitHub configuration in code.

It supports the creation of:
- Repositories
- Branch protection rules
- Actions permissions
- Teams
- Teams permissions

By using this module you ensure that:
- All repos are built to a standard pattern
- Repos are always delegated to teams
- Branch protection rules are configured in a standardised way
- GitHub actions permissions are configured to a baseline

## Example Usage

The module can be called from your Terraform as shown in this example below:

```

module "example" {
  source = "github.com/UKHomeOffice/ccoe-module-github?ref=v1.0.0"

  # ---------------------------------------------------------
  # Repositories
  # ---------------------------------------------------------

  repositories = {
    "public-cloud-github" = {
      name               = "public-cloud-github"
      description        = "Terraform management of the Public Cloud GitHub config."
      protected_branches = {
        "main" = {
          pattern = "main"
          checks  = ["Terraform"]
        }
      }
    }
    "ccoe-module-github" = {
      name        = "ccoe-module-github"
      description = "Module for managing GitHub config in code."
      visibility  = "public"
      protected_branches = {
        "main" = {
          pattern = "main"
        }
      }
    }
    "ccoe-prototype-2" = {
      name        = "ccoe-prototype-2"
      description = "This prototype is based on the layout of the GOV.UK Design System pages and using the Design System components - this version of our prototype has a different navigation"
      archived    = true
    }
  }

  # ---------------------------------------------------------
  # Teams
  # ---------------------------------------------------------

  teams = {
    "Admin" = {
      name        = "Admin"
      description = "Admin Team"
      permission  = "admin"
      members     = {
        "MikeHosker-HO" = {
          role = "maintainer"
        }
      }
    }
    "DevOps" = {
      name        = "DevOps"
      description = "DevOps Team"
      permission  = "push"
      members     = {
        "MikeHosker-HO" = {
          role = "maintainer"
        }
        "mhosker" = {
          role = "member"
        }
      }
    }
  }
}

```