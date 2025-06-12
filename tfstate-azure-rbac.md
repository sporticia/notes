Github Actions for storing Terraform state on Azure storage accounts now requires you to assign Storage Blob Data Owner (subscription Owner no longer has data rights!)

https://developer.hashicorp.com/terraform/language/backend/azurerm#storage-account-required-role-assignments

Don't forget to make sure to set the Github workflow to `ARM_USE_OIDC: true` or you get storage RBAC errors!
