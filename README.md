# AKS Traffic Routing using Traffic Manager Profiles
Quick demo of routing ingress traffic between two cluster ip services utilizing nginx ingress controller. Very basic - no annotations at this point.

## Install

From the root of this repo...

``` bash
kubectl apply -k .

helm repo add nginx-stable https://helm.nginx.com/stable
helm repo update

helm install nginx-inc nginx-stable/nginx-ingress -n nginx -f values.yaml
```

## Traffic Manager

1. Find the public ip address resource for your nginx ingress controller in your `MC_xxx` resource group.
1. Create an alias record for that public ip address. Go to Configuration under Settings for the Public IP Address and from there you can create an Alias ("A") record. In my case, I already had an Azure DNS Zone for `kubedave.com` set up, so creating an Alias record for this test was easy. If you don't have an Azure DNS Zone, you'll have to create one.
1. Create a Traffic Manager Profile of type Priority
1. Create an Endpoint for your profile, mapping it to your AKS public ip address for the NGINX Ingress Controller

## Test

1. Open up PostMan
1. URL should be for your Traffic Manager Profile name
1. Add a Host header value, setting `Host` to the value of one of your services, e.g. `gy-routing-sock.kubedave.com`.
1. Send the request - it should route to the correct ClusterIP service

