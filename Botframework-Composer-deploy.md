# Purpose:

I ran into some challenges when using [Azure Botframework Composer](https://github.com/Microsoft/BotFramework-Composer) when publishing my bot to existing Azure resources.  The Composer is a great tool and will handle the creation of resources in Azure via the provisionComposer.js. I was unable to use that script because it makes assumptions on how resources shoudl be in Azure that didn't align to what I require. So there is an option to publish to existing Azure resources but the documentation is unclear at best. 

The follow is the Publish configuration that worked for me:
* `name` and `environment` are required even if you are deploying to existing resources. If you do not define `hostname` then Composer will look for a Web App bot named ${name}-${dev} in Azure
* `endpoint` and `endpointKey` had to equal the `authoringKey` and `authoringEndpoint`. They also had to be dfined
* `luisResource` is where you define the actual Luis prediction endpoint

## Configuration Example
```jsonc
{
  "accessToken": "pwsh.exe -command (az account get-access-token -o tsv --query accessToken | Set-Clipboard)",
  "name": "bot001", //Name you want your Luis application named inside Luis portal - will be named bjdbot001(dev)-qnamakerluissample-0.en-us.lu
  "environment": "dev", //Environment marker for your Luis application inside Luis portal 
  "settings": {
    "luis": {
      "authoringKey": "", //Authoring Endpoint Key
      "authoringEndpoint": "https://example001-authoring.cognitiveservices.azure.com", //Luis Authoring endpoint
      "endpointKey": "", //Authoring Endpoint Key
      "endpoint": "https://example-authoring.cognitiveservices.azure.com", //Luis Authoring endpoint
      "region": "westus"
    },
    "MicrosoftAppId": "64d3ce26-ccba-47d2-****-************", //SPN for Azure Bot.
    "MicrosoftAppPassword": "RVN---------------------" //Password for the SPN
  },
  "hostname": "bot001",  //Name of the Web Application that will host bost code. Deploys to SCM console - bjdbot001.scm.azurewebsites.net
  "luisResource": "luis001", //Prediction Endpoint name that your LUIS appliciation will be published to
  "subscription": "5cd8aef3-0105-48cd-****-************" //Azure subscription where all the resources live
}
```

# To do:
* [ ] - Cosmos DB settings
* [ ] - Storage account setttings