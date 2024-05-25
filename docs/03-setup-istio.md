# Installing Istio

To install Istio, you need to install both the `istio-base` and `istiod` charts. Follow the steps below to install Istio using Helm.

## Step 1: Add the Istio Helm repository and create the namespace

```shell
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update
kubectl create ns istio-system
```

## Step 2: Install the Istio base chart

```shell
helm install istio-base istio/base -n istio-system
```

## Step 3: Install the Istio control plane

```shell
helm install istiod istio/istiod -n istio-system
```

## Step 4: Install ingress gateway (only for dev)
    
```shell
helm upgrade --install istio-ingressgateway istio/gateway -n dev --set service.type=ClusterIP
```

## Step 5: Modify your hosts file

1. Open the file `C:\Windows\System32\drivers\etc\hosts` in your favorite text editor (recommended VSCode).
2. Add the following line to the end of the file:
```
127.0.0.1 app.sample.com
127.0.0.1 app-qa.sample.com
```

## Step 6: Do port-forwarding

```shell
kubectl port-forward -n dev svc/istio-ingressgateway 8080:80 # For dev
kubectl port-forward -n qa svc/istio-ingressgateway 8081:80 # For qa
```


## Step 7. Reconcile the kustomization

```shell
flux reconcile kustomization apps-dev -n flux-system --with-source
flux reconcile kustomization apps-qa -n flux-system --with-source
```

## Step 8: Access the application

Open your web browser and navigate to [http://app.sample.com:8080](http://app.sample.com:8080)
or [http://app-qa.sample.com:8080](http://app-qa.sample.com:8080).
*Note: You may need to wait a few minutes for the application to be accessible.*


## Add prometheus
```shell
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.22/samples/addons/prometheus.yaml -n istio-system
```