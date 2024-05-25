# Canary experiments

The goal of this section is to demonstrate how to perform canary experiments using Flagger. We will deploy a sample application and configure Flagger to perform a canary deployment.

## Step 1: Modify the application version

1. Open the file `apps/common/release.yaml` in your repository and change the version from `6.0.0` to `6.0.1`.
2. Wait (about 5 minutes) for FluxCD to detect the changes and deploy the new version of the application. You can also execute the following command to force a sync:

```shell
flux reconcile source git flux-system
```
3. Describe the canary in the dev environment
    
```shell
kubectl describe canary -n dev
```

## Step 2 (In parallell after 1.1):

1. Open the application in your web browser and navigate to [http://app.sample.com:8080](http://app.sample.com:8080). Refresh the browser several times to see the different versions of the application version.