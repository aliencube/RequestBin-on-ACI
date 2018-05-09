# RequestBin on ACI #

This provides an ARM template to deploy Azure Container Instances (ACI) that runs a self-hosted [RequestBin](http://requestb.in/) application.


## Getting Started ##

This ARM template deploys the following Azure resources:

* Azure Storage Account
* Consumption Plan
* Azure Functions
* Azure Container Instances (ACI)

ACI hosts the RequestBin application by pulling out the [RequestBin image](https://hub.docker.com/r/crccheck/requestbin/) and [Redis image](https://hub.docker.com/_/redis/) from Docker Hub. As current ACI service doesn't support HTTPS connection, if HTTPS connection is necessary, the Azure Functions Proxy helps it.


### Deployment ###

Deployment steps below assumes using Azure PowerShell.

1. Login to Azure Resource Manager through PowerShell.

    ```powershell
    Login-AzureRmAccount
    ```
1. Create a resource group. Due to the restriction of ACI, its location needs to be considered to one of "East US", "West US", "West US 2", "North Europe", "West Europe", "Southeast Asia".

    ```powershell
    New-AzureRmResourceGroup `
        -Name "[RESOURCE_GROUP_NAME]" `
        -Location "[LOCATION]"
    ```

1. Update `azuredeploy.parameters.json`.
1. Deploy the ARM template. Once deployed, it will return both Azure Functions application URL and ACI endpoint URL.

    ```powershell
    New-AzureRmResourceGroupDeployment `
        -Name RequestBin `
        -ResourceGroupName "[RESOURCE_GROUP_NAME]" `
        -TemplateFile "azuredeploy.json" `
        -TemplateParameterFile "azuredeploy.parameters.json" `
        -Verbose
    ```

1. Deploy the Azure Function Proxy through Azure Portal.
1. Run both Azure Functions app and ACI container.


## Contribution ##

Your contributions are always welcome! All your work should be done in your forked repository. Once you finish your work with corresponding tests, please send us a pull request onto our `master` branch for review.


## License ##

**RequestBin on ACI** is released under [MIT License](http://opensource.org/licenses/MIT)

> The MIT License (MIT)
>
> Copyright (c) 2018 [aliencube.org](https://aliencube.org)
> 
> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
> 
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
> 
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
