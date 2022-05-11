# Manual steps
- Follow the OHIF viewer [build-for-production](https://docs.ohif.org/deployment/recipes/build-for-production.html) 
- Run `yarn run build:package` to create a module package
- Copy the build output except app-config.json to https://dcmcistorage.blob.core.windows.net/ohifbuild-05-10-2022 