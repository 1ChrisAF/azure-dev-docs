---
title: Configure an Azure Attestation provider using Terraform
description: Learn how to use Terraform to create an Attestation provider on Azure.
keywords: azure devops terraform attestation
ms.topic: how-to
ms.date: 08/07/2021
ms.custom: devx-track-terraform
---

# Configure an Azure Attestation provider using Terraform

[!INCLUDE [Terraform abstract](./includes/abstract.md)]

This article shows example Terraform code for creating an [Attestation provider](/azure/attestation/overview) on Azure.

In this article, you learn how to:
> [!div class="checklist"]

> * Configure an Azure Attestatoin provider

## 1. Configure your environment

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

[!INCLUDE [configure-terraform.md](includes/configure-terraform.md)]

- **Policy Signing Certificate:** A PEM file defines a set of trusted signing keys. As there are many scenarios in which to have a PEM file, this article assumes you have access to one. For example, you can download a PEM during the process of creating a virtual machine in the [Azure portal](https://portal.azure.com).

## 2. Implement the Terraform code

1. Create a directory in which to test the sample Terraform code and make it the current directory.

1. Create a file named `main.tf` and insert the following code:

    [!code-terraform[master](../../terraform_samples/quickstart/101-attestation-provider/main.tf)]

1. Create a file named `variables.tf` to contain the project variables and insert the following code:

    [!code-terraform[master](../../terraform_samples/quickstart/101-attestation-provider/variables.tf)]
    
    **Key points:**
    
    - Adjust the `policy_file` field as needed to point to your PEM file.
    
## 3. Initialize Terraform

[!INCLUDE [terraform-init.md](includes/terraform-init.md)]

## 4. Create a Terraform execution plan

[!INCLUDE [terraform-plan.md](includes/terraform-plan.md)]

## 5. Apply a Terraform execution plan

[!INCLUDE [terraform-apply-plan.md](includes/terraform-apply-plan.md)]

## 6. Clean up resources

[!INCLUDE [terraform-plan-destroy.md](includes/terraform-plan-destroy.md)]

## Troubleshoot Terraform on Azure

[Troubleshoot common problems when using Terraform on Azure](troubleshoot.md)

## Next steps

> [!div class="nextstepaction"] 
> [Learn more about using Terraform in Azure](/azure/terraform)