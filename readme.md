# Rattus

```Rattus``` is a lightweight credentials provisioning tool focused on:

- simplicity
- containers support
- repeatable workflow for every credentials provider
- fully configurable through environment variables or flags
- template support

```Rattus``` is designed, to provide the same workflow for credentials provisioning at different environments. 
For example, you have local development environment, that runs under the Kubernetes cluster and you store all your secrets at Vault or at K8S secrets.
But your production environment, deployed at AWS ECS and you can`t use same credential provisioning workflow at both environments.
```Rattus``` fixes that issue, and you can use the same command to retrieve credentials or generating configuration files in every environment.
```Rattus``` is designed for a be configured through environment variables. Because with environment variables you can easily change workflow at different environments, without changing application initialization logic.

# Usage example

Create a shell script, that will be launch at your application startup with followed content:

```bash
#!/bin/sh
/bin/rattus > /app/config.json
```

And that's all! ``Rattus`` will get credentials, render template file, and save the output to application config.
Now you can use this script in every environment, and you will get the same credentials provisioning workflow. All that you need to change - environment variables, that can be easily changed.

See more [examples](https://github.com/rma945/rattus/examples)

# Support credential providers

## Hashicorp Vault

Rattus support [Vault](https://github.com/hashicorp/vault) througt followed auth methods: 

- [kubernetes auth provider](https://www.vaultproject.io/docs/auth/kubernetes/)
- [tokens](https://www.vaultproject.io/docs/concepts/tokens/)

## AWS Secret manager

Rattus support [AWS secret manager](https://aws.amazon.com/secrets-manager/) throught:

- [AWS IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
- [AWS Credential](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html)

## Azure Key Vault

Rattus support [Azure Key Vault](https://azure.microsoft.com/en-us/services/key-vault/) througt followed auth methods: 

- [Azure Service provider credentials](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)
- [Azure Managed Service Identity](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview/)

# Configuration options

Rattus supports configuration through flags or through environment variables. The preferred way to work with Rattus - use environment variables, because in that case - you don't need to change the credentials initialization workflow for your application.

```
-aws-key-id string
  env: AWS_ACCESS_KEY_ID
  
-aws-key-secret string
  env: AWS_SECRET_ACCESS_KEY

-aws-session-token string
  env: AWS_SESSION_TOKEN
  
-aws-region string
  env: AWS_DEFAULT_REGION
  
-aws-secret-name string
  AWS secret name - example-project-backend
  env: AWS_SECRET_NAME
  
-azure-client-id string
  env: AZURE_CLIENT_ID
  
-azure-client-secret string
  env: AZURE_CLIENT_SECRET
  
-azure-tenant-id string
  env: AZURE_TENANT_ID
  
-azure-vault string
  Azure keyvault storage URL - https://example-key-vault.vault.azure.net/
  env: AZURE_VAULT
 
-template string
  Path to template file - /app/config/production.template
  env: TEMPLATE_PATH
  
-vault-secret string
  Vault secret URL - https://vault.example.io/v1/storage/secret
  env: VAULT_SECRET
  
-vault-token string
  Vault authentication token
  env: VAULT_TOKEN

-debug
    Enable debug information
```

# Template file example

```Rattus``` uses default [Golang template](https://golang.org/pkg/text/template/) syntax:

```bash
# generated by rattus {{datetime}}

APP_ENV={{$.APP_ENV}}
APP_DEBUG={{$.APP_DEBUG}}

DB_CONNECTION={{$.DB_CONNECTION}}
DB_HOST={{$.DB_CONNECTION}}
DB_PORT={{$.DB_CONNECTION}}
DB_DATABASE={{$.DB_DATABASE}}
DB_USERNAME={{$.DB_USERNAME}}
DB_PASSWORD={{$.DB_PASSWORD}}
```