## Create Azure Service Principal for Authentication

To make it easy for you to authenticate through a non-interactive session to Azure, I've built a script to create an Azure Service Principal.

## Pre-requisites

In order to run the script, you need the following installed:

- Azure CLI

## Service Principal Creation and Authentication

In order to login to Azure using a service principal, we use the following comand:

```
azure login -u $spn -p $password --tenant $tenant --service-principal
```

You can either pass the command manually, or you can include it in a shell script. However, in order for it to work, you first need to create the service principal.

In short, you need environment variables for the following:

```
spn=service_principal_name
password=service_principal_password
tenant=azure_tenant_id
```

To help you get started, I created the [Service Principal Creation Script](local_scripts/create_serviceprincipal.sh), which needs to be run on your local machine. You will need to have [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install) installed. 

To run the script, save it to the root of your repository and give it executable permissions:

```
chmod +x local_scripts/create_serviceprincipal.sh
```
The above example assumes you are in the root of your repo. You will want to adjust the file path accordingly. It is recommended to run this script from root since you might need the encrypted env files available at the root, unless you specify a different path in your codeship-services.yml file.

Then run the script by typing the following: 
```
. ./create_serviceprincipal.sh id_name_here password_here role_here
```
or
```
source ./create_serviceprincipal.sh id_name_here password_here role_here
```
```
### Description of positional parameters

id_name_here - Name of Service Principal (for your reference only)
password_here - Password for service principal created
role_here - Desired role see [RBAC: Built-in roles](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles)
```

Example:

```
. ./create_serviceprincipal.sh AzureDemo PasswOrd!! Contributor
```
NOTE: Your password needs to be a minimum of 12 characters and have some complexity incorporated to it. See more here: [Azure Password Policies](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-policy)

The spn creation script will create a service principal for you and assign it the role of Contributor. The script will then export the necessary environment variables for the below command to work.

```
azure login -u $spn -p $password --tenant $tenant --service-principal
```

## See Also:

- [Use Azure CLI to create a service principal to access resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
- [Use portal to create Active Directory application and service principal that can access resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)
- [Manage Role-Based Access Control with the Azure command-line interface](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli)
- [RBAC: Built-in roles](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles)




