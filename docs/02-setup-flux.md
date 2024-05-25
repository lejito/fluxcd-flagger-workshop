# Setting up FluxCD with Minikube

## Prerequisites
- Minikube cluster is already installed and running
- Linux-based machine

## Step 1: Install Flux CLI
1. Open a terminal window.
2. Run the following command to download the Flux CLI binary:
    ```shell
    curl -s https://fluxcd.io/install.sh | sudo bash
    sudo chmod +x /usr/local/bin/flux
    ```
3. Verify the installation by running the following command:
    ```shell
    flux --version
    ```
    You should see the version of Flux CLI printed on the console.

## Step 2: Generate GitHub Personal Access Token (PAT)
1. Open your web browser and go to GitHub.
2. Navigate to your GitHub profile settings.
3. Go to "Developer settings" > "Personal access tokens".
4. Click on "Generate new token".
5. Provide a meaningful name for the token and select the required scopes (repo full).
6. Click on "Generate token" and make a note of the generated token.

## Step 3: Change with your user configuration

```shell
export GITHUB_USER=<your-github-username>
sed -i "s/adgrajales1/$GITHUB_USER/g" ./clusters/minikube/flux-system/gotk-sync.yaml
git add -A
git commit -m "Change github user"
git push

kubectl create ns dev
kubectl create ns qa
kubectl label ns dev istio-injection=enabled
kubectl label ns qa istio-injection=enabled
```

## Step 3: Configure FluxCD
1. Open a terminal window.
2. Run the following command to set up the FluxCD namespace and configure it to use HTTPS + GitHub + GitHub PAT:
    ```shell
    export GITHUB_TOKEN=<your-github-token>
    export GITHUB_USER=<your-github-username>
    export GITHUB_REPO=fluxcd-flagger-workshop
    export GITHUB_BRANCH=main
    export FLUX_FORWARD_NAMESPACE=flux-system
    export CLUSTER_NAME=minikube

    flux bootstrap github \
      --token-auth \
      --owner=$GITHUB_USER \
      --repository=$GITHUB_REPO \
      --branch=$GITHUB_BRANCH \
      --path=./clusters/$CLUSTER_NAME \
      --personal \
      --namespace=$FLUX_FORWARD_NAMESPACE
    ```
    Replace the placeholders with your actual values.

3. FluxCD will now sync with your GitHub repository and start deploying the defined resources to your Minikube cluster.

## Step 4: Verify FluxCD Setup
1. Run the following command to check the status of FluxCD:
    ```shell
    flux check
    ```
    You should see a summary of the resources that FluxCD has deployed.

2. To view the deployed resources, run the following command:
    ```shell
    kubectl get all -n $FLUX_FORWARD_NAMESPACE
    ```
    You should see a list of resources deployed by FluxCD.
3. Execute reconcile process to validate the setup:
    ```shell
    flux reconcile source git flux-system
    ```
    You should see a list of resources deployed by FluxCD.

Congratulations! You have successfully set up FluxCD with Minikube using HTTPS + GitHub + GitHub PAT.
