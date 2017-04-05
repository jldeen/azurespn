## Create Azure CLI 2.0 Service Principal for Authentication

To make it easy for you to deploy your application to Azure, I've built a script to create an Azure Service Principal (2.0).

## Pre-requisites

In order to run the script, you need the following installed:

- Azure CLI 2.0
- JQ (1.5 or higher)

## Service Principal Creation and Authentication

In order to login to Azure using a service principal, we use the following comand:

```
az login \
        --service-principal \
        -u $spn \
        -p $password \
        --tenant $tenant
```

You can either pass the command manually, or you can include it in a shell script. However, in order for it to work, you first need to create the service principal.

In short, you need environment variables for the following:

```
spn=service_principal_name
password=service_principal_password
tenant=azure_tenant_id
```

To help you get started, I have created a [Service Principal Creation Script](local_scripts/create_serviceprincipal.sh), which needs to be run on your local machine. You will also need to have [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) installed. 

To run the script save it to the root of your repository and give it executable permissions:

```
chmod +x local_scripts/create_serviceprincipal.sh
```
The above example assumes you are in the root of your repo. You will want to adjust the file path accordingly.

Then run the script: 
```
./create_serviceprincipal.sh
```
or
```
local_scripts/create_serviceprincipal.sh
```

The script will prompt you for a name, password and role. A description of each is below: 

### Description of prompts 

- name - Name of Service Principal (for your reference only)

- password - Password for service principal created

- role - Desired role see [RBAC: Built-in roles](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles)

NOTE: Your password needs to be a minimum of 12 characters and have some complexity incorporated to it. See more here: [Azure Password Policies](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-policy)

The script will check for multiple subscriptions on your account, if found, you will be asked to select which subscription you wish to use prior to proceeding. The spn creation script will create a service principal for you and assign it the role you designate. 

The script will then export the necessary environment variables to ~/.bashrc for the below command to work. You may need to reload your ~/.bashrc profile first by doing the following:

```
source ~/.bashrc
```
or
```
. ~/.bashrc
```
After you reload your bashrc profile, you will be able to create your non-interactive authenticated session to Azure using the command below:

```
az login \
        --service-principal \
        -u $spn \
        -p $password \
        --tenant $tenant
```

## See Also:

- [Create an Azure service principal with Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli)
- [Use portal to create Active Directory application and service principal that can access resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)
- [Manage Role-Based Access Control with the Azure command-line interface](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli)
- [RBAC: Built-in roles](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles)