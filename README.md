# Azure DICOM service with OHIF viewer

This project provides guidence on deployment of [OHIF Viewer](https://ohif.org/) on Azure and configurations needed to work with Azure Health Dicom service .

OHIF is a open source non-diagnostic viewer that uses DICOMWeb API's to find and render DICOM images.

## Setup
### Create a new Azure Health Data DICOM service
- Create a Azure Health Data services workspace
- Create a DICOM service
- Enable RBAC
- Enable CORs
- Remember the `DICOM service Url`

### Create a new AAD Application Client
- Create a new AAD App registration
- Set Authentication with Static websites with callback url and enable ID_Token and Token Auth
- Add API Permission on "Azure API for Dicom" + ReadWrite
- Grant Admin consent on the new API Permission
- Remember the `Application\Client ID`

### Deploy OHIF on Azure Storage Static Website 

- <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmicrosoft%2Fdicom-server%2Fmain%2Fsamples%2Ftemplates%2Fdefault-azuredeploy.json" target="_blank"><img src="https://aka.ms/deploytoazurebutton"/></a>

```cmd
# Copy Static website content
blobUrl="https://$storageAccountName.blob.core.windows.net/\$web/"
azcopy rm $blobUrl --recursive=true --include-pattern="*"
azcopy copy "build/*" $blobUrl --recursive

# Ensure static webhosting is enabled
az storage blob service-properties update --static-website --index-document "index.html" --account-name $storageAccountName --auth-mode login
```
- Update properties in app.config. 
- Browse to the blobUrl to access OHIF viewer


You can do additional Domain and CDN configurations as need.

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
