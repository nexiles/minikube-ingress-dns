# Minikube DNS

DNS service for ingress controllers running on your minikube server

## Overview

### Problem
When running minikube locally you are highly likely to want to run your services on an ingress controller so that you don't have to do anything weird like use minikube tunnel or something similiar. Ingress controllers are great because you can define your entire architecture in something like a helm chart and all your services will be available. There is only 1 problem. That is that your ingress controller works basically off of dns and while running minikube that means that your local dns names like `local.service` will have to resolve to `$(minikube ip)` not really a big deal except the only real way to do this is to add an entry for every service in your `/etc/hosts` file. This gets messy for obvious reasons. If you have a lot of services running that each have their own dns entry then you have to set those up manually. Even if you automate it you then need to rely on the host operating system storing configurations instead of storing them in your cluster. To make it worse it has to be constantly maintained and updated as services are added, remove, and renamed. I call it the `/ets/hosts` pollution problem.

### Solution
What if you could just access your local services magically without having to edit your `/etc/hosts` file? Well now you can. This project acts as a DNS service that runs inside your kubernetes cluster. All you have to do is install the service and add the `$(minikube ip)` as a DNS server on your host machine. 

## Installation

### Pre-requisites
Helm is the easiest way to install the service. Install helm first. https://helm.sh/

### Nginx 
This is tested using the minikube ingress plugin as well as traefik. If you use some other form of ingress controller your settings to set external routing for tcp and udp services on port 53 might be different. TCP and UDP routing for the DNS service is roughly based on the concepts in this article https://kubernetes.github.io/ingress-nginx/user-guide/exposing-tcp-udp-services/

### Install the service with helm
```bash
git clone git@gitlab.com:cryptexlabs/public/development/minikube-dns.git
cd minikube-dns
./k8s/bin/install.sh
```

### Customizing tcp and udp services
Note: This updates the tcp-services and udp-services config maps. You may need to customize them if you have your own tcp and udp services you are exposing through an ingress controller. 