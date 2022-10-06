# CloudBees_CI_Tanzu_Support
Integrating the CloudBees CI with VMware Tanzu Kubernetes Grid (with Avi as load balancer and ingress controller)

## Problem to solve

Basically Tanzu Kubernetes Grid is a commercial offering of Kubernetes which is CNCF compliant, which means that Cloudbees CI could be installed on it with Nginx as Ingress Controller. 

However, since AVI is the default load balancer and ingress controller for VMware Tanzu product and VMware is leveraging AVI for more advanced features, it is possible that the usage of AVI on Tanzu is enforced in some organization and Cloudbees CI may need to integrate with AVI in this case. 

## Solution

Although not tested, the CloudBees CI is actually capable of integrating with quite some other major ingress controllers such as Contour. 

However, as for the AVI ingress controller in TKG, the default helm chart for Cloudbees CI installation does not work because the current version of AVI ingress controller does not support ClusterIP which is the default service type for both cjoc and controllers. 

Therefore, to integrate the CloudBees CI with AVI, we need to change ingress class to avi and change the service type from ClusterIP to Nodeport. 

## Steps

In the CloudBees CI values.yaml file: 

1. Set the ingress class to "avi";
2. Set the service type of operation center to "Nodeport";

3. Install the CloudBees CI without nginx ingress enabled;

In CJOC Dashboard -> Manage Jenkins -> Configure Controller Provisioning:

4. Set the "Ingress Class" to "avi"; 
5. In "YAML", input the following code:

```
apiVersion: "v1"
kind: "Service"
spec:
  type: NodePort
```



