# Setting up the Kubernetes Cluster with Minikube

## Prerequisites (Optional if using Codespaces)
- Docker is already installed on your machine.

## Step 1: Install Minikube (Not needed if using Codespaces)
1. Refer to the [Minikube installation guide](https://minikube.sigs.k8s.io/docs/start/) to install Minikube on your machine.

## Step 2: Start Minikube
1. Run the following command to start Minikube:
    ```
    minikube start --memory=4gb --cpus=2
    ```

## Step 3: Verify Minikube Installation
1. Run the following command to verify that Minikube is running:
    ```
    minikube status
    ```

Congratulations! You have successfully set up a Minikube cluster with Docker installed.

## Next Steps
Now that you have a running Minikube cluster, you can start deploying and managing your Kubernetes applications.
