---
title: Remove Linux virtual machine resource
description: Clean up Azure resources by removing the resource group with an Azure CLI command. 
ms.topic: how-to
ms.date: 01/18/2022
ms.custom: devx-track-js, devx-track-azurecli
---

# 7. Clean up resources

Once you have completed this tutorial, you need to remove the resource group, which includes all its resources to make sure you are not billed for any more usage. 

## Remove all the resources by removing resource group

In the same terminal, use the Azure CLI command, [az group delete](/cli/azure/group#az-group-delete), to delete the resource group:

```azurecli
az group delete --name rg-demo-vm-eastus -y
```

This command takes a few minutes. 

## Next step

* Learn more about [Azure Linux VMs](/azure/virtual-machines)
