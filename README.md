Networking-and-Service-Exposure-Mastery-in-OpenShift

Objectives
Understand internal and external networking in OpenShift.
Configure and manage services and routes.
Expose applications securely to the outside world.


Tasks
1. 
a) Create and Configure a Service of Type ClusterIP, & NodePort
oc create -f lb8clusterip.yaml

b) Create a nodeport service yaml file, name is lb8nodeport.yaml 
oc create -f lb8nodeport.yaml

2. Create a Route to Expose a Service Outside the Cluster
a) Create a Route for the Service:
oc create -f lb8routeforservice.yaml

b) Secure a Route Using TLS and Understand the Basics of Network Policies
oc create -f lb8securingroutefortls.yaml

c) Create a Basic Network Policy:
oc create -f lb8networkpolicy.yaml


You can check them using these commands: 

oc get svc
oc get routes
oc get networkpolicy
