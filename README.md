# Cnvrg Kubernetes Chart



Use this repo to install cnvrg on your kubernetes cluster

**Usage**

    helm template --namespace cnvrg --name cnvrg . -f values.yaml > all.yaml
    kubectl apply -f pre-crds.yaml
    kubectl apply -f all.yaml



## Chart options
| **key**              | **default value** | **description**
| ----------| :------|:--- |
| global.namespace|cnvrg| Namespace to install components| istio.enabled | true | Install istio for inbound traffic
| istio.serviceType | LoadBalancer | Whether istio will use NodePort or Load Balancer for exposing its services
| cert-manager.enabled | true | Use cert manager to sign this domain (using route53)
| prometheus.enabled | true | Install prometheus for cluster monitoring
| grafana.enabled | false | Install grafana for monitoring visualization
| kubernetes-dashboard.enabled | false | Install kubernetes dashboard for cluster visualization
| elasticsearch.enabled | true | Install elastic search for logs
| fluentd.enabled | true | Run fluentd for logs collecting
| elastalert.enabled | true | Install elastalert for logs alerts
| kibana.enabled | true | Install kibana to view elasticsearch logs
| nvidia-device-plugin.enabled | false | Install nvidia device plugin for installing and mounting gpu devices

## Install with minikube
To set up cnvrg.io on a Minikube cluster, follow these instructions:

### Prerequisites

1. Minikube (`brew install minikube`)
2. Helm (2.16)

### Steps

1. Prepare the chart yaml file
    `helm template --namespace cnvrg --name cnvrg . -f minikube.yaml > minikube-full.yaml`
2. Launch the minikube cluster
    `minikube start --disk-size=30g --memory=8000 --mount-string="source_data_path:/data"`
3. Set labels to nodes:
     `kubectl label node minikube cnvrg.io/app=server`
     `kubectl label node minikube cnvrg.io/db=server`
4. Apply!
    `kubectl apply -f minikube-full.yaml`
5. Then get the minikube ip:
    `$ minikube ip`
    and use the default port: `30080` to go live
