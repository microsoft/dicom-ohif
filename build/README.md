## How to update ohif.zip

- Make sure you have npm installed
- Run `npm view @ohif/app@3.7.0 dist.tarball` to get the url of the latest version of the OHIF viewer. Replace `3.6.0` with the latest version of the OHIF viewer. You can find the latest the version of the OHIF viewer [here](https://www.npmjs.com/package/@ohif/app?activeTab=versions).
- Download the tarball from the url returned by the previous command.
- Extract the tarball.
- Compare the contents of the \app-3.6.0\package\dist\app-config.js with the \build\app-config.js. Ensure the dicomweb configuration is the same and add the oidc attributes to the \app-3.6.0\package\dist\app-config.js if they are missing.

Below is the sample oidc configuration and data source configuration for Azure DICOM service:

```json
dataSources: [
    {
      namespace: '@ohif/extension-default.dataSourcesModule.dicomweb',
      sourceName: 'dicomweb',
      configuration: {
        friendlyName: 'Azure Dicom Web',
        name: 'AzureDicomWeb',
        wadoUriRoot: '%dicom-service-url%/v1',
        qidoRoot: '%dicom-service-url%/v1',
        wadoRoot: '%dicom-service-url%/v1',
        qidoSupportsIncludeField: true,
        supportsReject: false,
        imageRendering: 'wadors',
        thumbnailRendering: 'wadors',
        enableStudyLazyLoad: true,
        supportsFuzzyMatching: true,
        supportsWildcard: true,
        staticWado: true,
        singlepart: 'bulkdata,video',
        // whether the data source should use retrieveBulkData to grab metadata,
        // and in case of relative path, what would it be relative to, options
        // are in the series level or study level (some servers like series some study)
        bulkDataURI: {
          enabled: true,
          relativeResolution: 'studies',
        },
        omitQuotationForMultipartRequest: true,
      },
    }
]
```

```json
oidc: [
    {
      // ~ REQUIRED
      // Authorization Server URL
      authority: 'https://login.microsoftonline.com/%aad-tenant-id%/v2.0/',
      client_id: '%application-client-id%',
      redirect_uri: '/callback', // `OHIFStandaloneViewer.js`
      response_type: 'token', // "implicit"
      scope: 'openid https://dicom.healthcareapis.azure.com/Dicom.ReadWrite', // https://dicom.healthcareapis.azure.com/Dicom.ReadWrite // email profile openid  https://dicom.healthcareapis.azure.com
      // ~ OPTIONAL
      post_logout_redirect_uri: '/logout-redirect.html',
      automaticSilentRenew: true,
      revokeAccessTokenOnSignout: true
    },
  ],
```
- Zip the dist folder and rename it to ohif.zip. Update the current zip in the repo.

## How to upload ohif.zip to existing account
- Take the contents of the ohif.zip and replace the contents in the storage account.
- The static web files are placed in $web container.
- Make sure the app.config.js is correctly set. Only on the first installation, the value of the dicom service url and oidc parameters are replaced, if you update manually, ensure the values are set correctly.


