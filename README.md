
# Terraform Azure Windwos Virtual Machine Deployment

## Hey there!

Welcome to my Terraform Azure Virtual Machine Deployment. In this guide, I'll help you set up and deploy resources on Azure using Terraform. I’ve included configurations here for creating a Virtual Machine (VM) in Azure, and I'll walk you through the steps to get it running.

## What’s in This Repo

Here’s what you’ll find in this repository:

- **`main.tf`**: This is where the magic happens. It contains all the Terraform code to create your Azure resources, like the VM, virtual network, and more.
- **`variables.tf`**: This file lists all the variables you can tweak, like VM size or location.
- **`terraform.tfvars`**: Here’s where you put your specific values for the variables, like your admin password.
- **`outputs.tf`**: This file tells Terraform what details to show you after deployment, like the public IP of your VM.
- **`.gitignore`**: This ensures sensitive files, like `terraform.tfvars`, aren’t pushed to GitHub.

## Getting Started

Here’s how to get everything set up and running:

### 1. Clone the Repository

First, grab a copy of the repo to your local machine:

```bash
git clone https://github.com/RizwanSid7/my-terraform-azure.git
cd my-terraform-azure
```

### 2. Set Up Azure Credentials

Make sure you’re logged into Azure. Run:

```bash
az login
```

If you have multiple subscriptions, you might need to set the right one:

```bash
az account set --subscription "your-subscription-id"
```

### 3. Configure Your Variables

Edit the `terraform.tfvars` file to set your specific values. This is where you’ll put things like your VM’s admin username and password. Make sure this file is kept secure!

Here’s an example of what `terraform.tfvars` might look like:

```hcl
prefix        = "myvm"
location      = "West Europe"
admin_username = "adminuser"
admin_password = "P@ssw0rd1234!"
```

### 4. Initialize Terraform

Get Terraform ready to go by running:

```bash
terraform init
```

### 5. Review the Plan

Check out what Terraform is going to do before it actually does it:

```bash
terraform plan
```

### 6. Apply the Configuration

Let Terraform create or update your resources. Confirm with `yes` when it asks:

```bash
terraform apply
```

### 7. Verify Everything

Head over to the [Azure Portal](https://portal.azure.com) to check that your resources are up and running.

### 8. Tear Down Resources

When you’re done, you can clean up everything with:

```bash
terraform destroy
```

Confirm with `yes` when prompted.

Configuration Files
main.tf
This is your main configuration file where you define things like your resource group, virtual network, network interface, and the VM itself.

variables.tf
Lists all the variables you can set, like VM size and location. These are used in main.tf.

terraform.tfvars
This file contains your actual values for the variables. It’s where you specify things like passwords and usernames.

outputs.tf
Defines what Terraform will output after deployment, like the VM’s public IP address.

Example for Linux VM
If you want to deploy a Linux VM instead of a Windows VM, just update main.tf to something like this:

hcl
Copy code
resource "azurerm_virtual_machine" "example" {
  name                  = "${var.prefix}-vm"
  location              = var.location
  resource_group_name   = azurerm_resource_group.example.name
  network_interface_ids = [azurerm_network_interface.example.id]
  vm_size               = "Standard_DS1_v2"

  storage_os_disk {
    name              = "myosdisk1"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "20.04-LTS"
    version   = "latest"
  }

  os_profile {
    computer_name  = "hostname"
    admin_username = var.admin_username
    admin_password = var.admin_password
  }

  os_profile_linux_config {
    disable_password_authentication = false
  }

  tags = {
    environment = "testing"
  }
}

Troubleshooting

Authentication Issues: Make sure you’re logged into Azure CLI and the right subscription is set.
Large Files: If you run into problems with large files, consider using Git Large File Storage (LFS) or avoid committing large binaries.

Contributing

Got ideas for improvements or found a bug? Feel free to open an issue or submit a pull request!
