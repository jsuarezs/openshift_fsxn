# Initial setup

These are the steps to start working with ROSA and AWS FSx for ONTAP as persistent storage:

- Deploy AWS FSx for ONTAP as persistent storage service for ROSA workloads. 
- Enable ROSA service in AWS console.
- Setting up ROSA cluster.
- Configuring IDP to authenticate against ROSA cluster as cluster-admin.
- Deploy NetApp Astra Trident inside ROSA in order to orchestrate Persistent Volumes (PVs) in AWS FSx for ONTAP created by Persistent Volumes Claims (PVC) from k8s.

## Enable ROSA in AWS Console

