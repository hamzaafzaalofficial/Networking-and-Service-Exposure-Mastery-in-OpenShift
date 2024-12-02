# Networking-and-Service-Exposure-Mastery-in-OpenShift

Objectives
Understand internal and external networking in OpenShift.
Configure and manage services and routes.
Expose applications securely to the outside world.



Tasks
1. Create and Configure a Service of Type ClusterIP, & NodePort
a) Creating a ClusterIP service yaml file name it lb8clusterip.yaml

apiVersion: v1
kind: Service
metadata:
  name: back-end
spec:
  type: ClusterIP
  ports:
    - port: 8000
      targetPort: 8080
  selector:
    app: backend 
oc create -f lb8clusterip.yaml


b) Create a nodeport service yaml file, name is lb8nodeport.yaml 

apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports:
    - targetPort: 8080
      port: 8080
      nodePort: 30008
  selector:
    app: front-end 
oc create -f lb8nodeport.yaml
2. Create a Route to Expose a Service Outside the Cluster
Objective
Instruct participants on creating a route to expose a service externally.

Steps
a. Create a Route for the Service:

Name it lb8routeforservice.yaml

apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: my-route
spec:
  to:
    kind: Service
    name: myapp-service  
  port:
    targetPort: 8080 
oc create -f lb8routeforservice.yaml
Secure a Route Using TLS and Understand the Basics of Network Policies

Steps

a. Secure a Route with TLS:



apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: my-route
spec:
  to:
    kind: Service
    name: myapp-service  
  port:
    targetPort: 8080
  tls:
    termination: edge
    key: |
      -----BEGIN PRIVATE KEY-----
      MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCyvJ8k5Q1b...
      -----END PRIVATE KEY-----
    certificate: |
      -----BEGIN CERTIFICATE-----
      MIIDdzCCAl+gAwIBAgIEbG9uZzANBgkqhkiG9w0BAQsFADBoMQswCQYDVQQGEwJV...
      -----END CERTIFICATE-----
    caCertificate: |
      -----BEGIN CERTIFICATE-----
      MIIDdzCCAl+gAwIBAgIEbG9uZzANBgkqhkiG9w0BAQsFADBoMQswCQYDVQQGEwJV...
      -----END CERTIFICATE----- 
oc create -f lb8securingroutefortls.yaml
b. Create a Basic Network Policy:



apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: basic-policy
spec:
  podSelector:
    matchLabels:
      app: frontend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: backend
    ports:
    - protocol: TCP
      port: 80
oc create -f lb8networkpolicy.yaml


You can check them using these commands: 

oc get svc
oc get routes
oc get networkpolicy
