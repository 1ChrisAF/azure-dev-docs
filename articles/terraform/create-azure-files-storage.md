---
title: Configure Azure Files for FSLogix profiles for Azure Virtual Desktop using Terraform - Azure
description: Learn how to use Terraform to configure Azure Files for FSLogix profiles Azure Virtual Desktop with Terraform
keywords: azure devops terraform avd virtual desktop storage fslogix
ms.topic: how-to
ms.date: 12/30/2021
ms.custom: devx-track-terraform
---

# Configure Azure Files using Terraform

Azure offers multiple storage solutions that you can use to store your FSLogix profile container. This article covers configuring Azure Files storage solutions for Azure Virtual Desktop FSLogix user profile containers using Terraform

In this article, you learn how to:
> [!div class="checklist"]

> * Use Terraform to Azure File Storage account
> * Use Terraform to configure File Share
> * Use Terraform to configure RBAC permission on Azure File Storage

## 1. Configure your environment

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

[!INCLUDE [configure-terraform.md](includes/configure-terraform.md)]

## 2. Define providers and create resource group

The following code defines the Azure Terraform provider:

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~>2.0"
    }
  }
}
provider "azurerm" {
  features {}
}
```

The following section creates a resource group in the location:

```hcl
resource "azurerm_resource_group" "rg_storage" {
  location = var.location
  name     = var.prefix-storage-rg"
}
```

In other sections, you reference the resource group with `azurerm_resource_group.rg_storage.name`.

## 3. Configure a File Storage Account

```hcl
resource "azurerm_storage_account" "storage" {
  name                     = "stor${random_string.random.id}"
  resource_group_name      = azurerm_resource_group.rg_storage.name
  location                 = azurerm_resource_group.rg_storage.location
  account_tier             = "Premium"
  account_replication_type = "LRS"
  account_kind             = "FileStorage"
  tags                     = "Terraform Demo"
}
```

## 4. Configure a File Share

```hcl
resource "azurerm_storage_share" "<FSShare>" {
  name                 = "fslogix"
  storage_account_name = azurerm_storage_account.storage.name
  depends_on           = [azurerm_storage_account.storage]
}

output "storage_account_name" {
  value = azurerm_storage_account.storage.name

}
```

## 5. Configure RBAC permission on Azure File Storage

```hcl
data "azurerm_role_definition" "storage_role" {
  name = "Storage File Data SMB Share Contributor"
}

resource "azurerm_role_assignment" "af_role" {
  scope              = azurerm_storage_account.storage.id
  role_definition_id = data.azurerm_role_definition.storage_role.id
  principal_id       = azuread_group.aad_group.id
}
```

## 6. Implement the Terraform code

To bring all these sections together and see Terraform in action, create a directory in which to test and run the sample Terraform code and make it the current directory.

1. Create a file named `main.tf` and insert the following code:

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~>2.0"
    }
  }
}
provider "azurerm" {
  features {}
}

## Create a Resource Group for Storage
resource "azurerm_resource_group" "rg_storage" {
  location = var.location
  name     = var.prefix-storage-rg"
}

## Create a File Storage Account 
resource "azurerm_storage_account" "storage" {
  name                     = "stor${random_string.random.id}"
  resource_group_name      = azurerm_resource_group.rg_storage.name
  location                 = azurerm_resource_group.rg_storage.location
  account_tier             = "Premium"
  account_replication_type = "LRS"
  account_kind             = "FileStorage"
}

resource "azurerm_storage_share" "FSShare" {
  name                 = "fslogix"
  storage_account_name = azurerm_storage_account.storage.name
  depends_on           = [azurerm_storage_account.storage]
}

output "storage_account_name" {
  value = azurerm_storage_account.storage.name

}

data "azurerm_role_definition" "storage_role" {
  name = "Storage File Data SMB Share Contributor"
}

resource "azurerm_role_assignment" "af_role" {
  scope              = azurerm_storage_account.storage.id
  role_definition_id = data.azurerm_role_definition.storage_role.id
  principal_id       = azuread_group.aad_group.id
}
```

## 7. Initialize Terraform

[!INCLUDE [terraform-init.md](includes/terraform-init.md)]

## 8. Create a Terraform execution plan

[!INCLUDE [terraform-plan.md](includes/terraform-plan.md)]

## 9. Apply a Terraform execution plan

[!INCLUDE [terraform-apply-plan.md](includes/terraform-apply-plan.md)]

## 10. Clean up resources

[!INCLUDE [terraform-plan-destroy.md](includes/terraform-plan-destroy.md)]

## Troubleshoot Terraform on Azure

[Troubleshoot common problems when using Terraform on Azure](troubleshoot.md)

## Next steps

> [!div class="nextstepaction"]
> [Learn more about using Terraform in Azure](/azure/terraform)
