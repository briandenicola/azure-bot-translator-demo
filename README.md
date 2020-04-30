# Introduction

A simple Bot based on the Bot Framework translates English to Italian

# Setup
## Manual Steps
* Create Azure AD Service Principal for Bot
    * $botPass = New-Password -Length 25 -ExcludeSpecialCharacters (Function from bjd.Common.Functions)
    * az ad app create --display-name bjdBotApp01 --password $botPass --available-to-other-tenants  --query 'appId' -o tsv

## ARM Template 
* cd infrastructure 
* az group create --name BOT_RG --location southcentralus
* az group deployment create --name bot -g BOT_RG --parameters `@azuredeploy.parameters.json --template-file .\azuredeploy.json --parameters botApplicationId=$botAppId botApplicationSecret=$botPass --verbose

# Deploy Bot Code
* Command Line
    * cd src
    * Change {{REPLACEME}} in src\ComposerDialog\translate\translate.dialog with Translator API Key
        * TBD to automate this with a App Settings variable 
    * dotnet build
    * dotnet publish -o publish
    * Compress-Archive -Path .\publish\* -DestinationPath bot.zip
    * az webapp deployment source config-zip --resource-group BOT_RG --name bjdtranslator --src .\bot.zip

* Azure DevOps Pipeline 
    * Create new pipeline from deploy\azure-pipeline.yaml
    * Update Variables for Service Connection and Azure App Service Name 

