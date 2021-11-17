# Initial setup

These are the steps to start working with ROSA and AWS FSx for ONTAP as persistent storage:

- Deploy AWS FSx for ONTAP as persistent storage service for ROSA workloads. 
- Enable ROSA service in AWS console.
- Setting up ROSA cluster.
- Configuring IDP to authenticate against ROSA cluster as cluster-admin.
- Deploy NetApp Astra Trident inside ROSA in order to orchestrate Persistent Volumes (PVs) in AWS FSx for ONTAP created by Persistent Volumes Claims (PVC) from k8s.

## Deploy AWS FSx for ONTAP




## Enable ROSA in AWS Console

Once the service is enabled the option to download ROSA CLI is active to deploy ROSA cluster by ROSA commands.

![Screen Shot 2021-10-28 at 10 25 40](https://user-images.githubusercontent.com/59535705/141461914-4a0a2d73-6318-4e37-9b91-d72493645c57.png)
