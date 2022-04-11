---
ms.author: tarcher
ms.topic: include
ms.date: 01/04/2022
ms.custom: devx-track-terraform
---

When you log in to the Azure portal with a Microsoft account, the default Azure subscription for that account is used.

Terraform automatically authenticates using information from the default Azure subscription.

Run [az account show](/cli/azure/account?#az-account-show) to verify the current Microsoft account and Azure subscription.

```azurecli
az account show
```

Any changes you make via Terraform will be against the displayed Azure subscription. If that's what you want, skip the rest of this article.
