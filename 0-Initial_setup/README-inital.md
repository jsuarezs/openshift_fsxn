# Initial setup

These are the steps to start working with ROSA and AWS FSx for ONTAP as persistent storage:

- Deploy AWS FSx for ONTAP as persistent storage service for ROSA workloads. 
- Enable ROSA service in AWS console.
- Setting up ROSA cluster.
- Configuring IDP to authenticate against ROSA cluster as cluster-admin.
- Deploy NetApp Astra Trident inside ROSA in order to orchestrate Persistent Volumes (PVs) in AWS FSx for ONTAP created by Persistent Volumes Claims (PVC) from k8s.

## Deploying AWS FSx for ONTAP

AWS FSx for ONTAP is a native AWS service so in order to deploy the service is as simple as accesing to AWS console within a privileged user to deploy FSx for ONTAP in just several clicks: [Getting started.](https://docs.aws.amazon.com/fsx/latest/ONTAPGuide/getting-started.html)

<img width="1320" alt="Screen Shot 2021-11-12 at 12 59 52" src="https://user-images.githubusercontent.com/59535705/142220610-1aa18f6e-c036-4c54-8bd1-5f0cac2ec31b.png">


## Enable ROSA in AWS Console

Once the service is enabled the option to download ROSA CLI is active to deploy ROSA cluster by ROSA commands.

![Screen Shot 2021-10-28 at 10 25 40](https://user-images.githubusercontent.com/59535705/141461914-4a0a2d73-6318-4e37-9b91-d72493645c57.png)

## Deploying ROSA cluster using ROSA CLI

Once ROSA CLI is downloaded and installed you have to take take of ROSA prerequisites to go ahead [Getting started.](hhttps://docs.openshift.com/rosa/rosa_getting_started/rosa-getting-started-workflow.html)

For testing purposes ROSA cluster is deployed in one AZ in this guide. One oublic subnet is also necessary to manage the cluster from RedHat console.

```
javiersuarez@jsuarezs-mac-0 rosa % ./rosa create cluster --cluster-name=pcsemeaos --subnet-ids=subnet-PUBLICID,subnet-PRIVATEID —compute-machine-type=m5.xlarge
I: Creating cluster 'pcsemeaos'
I: To view a list of clusters and their status, run 'rosa list clusters'
I: Cluster 'pcsemeaos' has been created.
```
Once the deployment is finished is shown as ready:

```
javiersuarez@jsuarezs-mac-0 rosa % ./rosa list clusters
ID                                NAME       STATE
1o2lvolnu884vg10ot7jdg88edco8a7n  pcsemeaos  ready
```

## Configure IDP to authenticate and manage the cluster

ROSA uses cluster authentication by IDP so in order to access to the cluster console is necessary to configure at least one authentication method. In this guide the IDP configuration was done by GitHub but more methods are availble to configure this step.

[ROSA IDP configuration guide.](https://docs.openshift.com/rosa/rosa_getting_started/rosa-config-identity-providers.html)

![Screen Shot 2021-10-26 at 11 21 40](https://user-images.githubusercontent.com/59535705/142592282-2b04bfaa-d5cc-416a-95eb-7c78537d2a31.png)

Once everything is ready and configured it's possible to get the OC login commando from RedHat console and manage ROSA as other k8s distributions.

````
[ec2-user@ip-10-0-1-145 ~]$ ./oc login --token=sha256~HxtGWbZ4_jqFw5Qm8JoCCJESblmP3AqLHKxxxxxx --server=https://api.pcsemeaos.njkf.p1.openshiftapps.com:6443
Logged into "https://api.pcsemeaos.xxxx.xx.openshiftapps.com:6443" as "jsuarezs" using the token provided.

You have access to 90 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".
Welcome! See 'oc help' to get started.
`````

Now OC command works to manage ROSA cluster and get info regarding the nodes. By default, ROSA deployment creates 3 Masters, 2 Infra workers and 2 workers nodes. The best practice is deploying NetApp Astra Trident in Infra nodes to separate maintenance and management tasks.

But in this guide Astra Trident is allowed on every node.

```
[ec2-user@ip-10-0-1-145 ~]$ ./oc get nodes
NAME                                          STATUS   ROLES          AGE   VERSION
ip-10-0-4-158.eu-central-1.compute.internal   Ready    infra,worker   60m   v1.22.0-rc.0+894a78b
ip-10-0-4-198.eu-central-1.compute.internal   Ready    infra,worker   60m   v1.22.0-rc.0+894a78b
ip-10-0-4-219.eu-central-1.compute.internal   Ready    worker         78m   v1.22.0-rc.0+894a78b
ip-10-0-4-247.eu-central-1.compute.internal   Ready    master         88m   v1.22.0-rc.0+894a78b
ip-10-0-4-254.eu-central-1.compute.internal   Ready    worker         78m   v1.22.0-rc.0+894a78b
ip-10-0-4-70.eu-central-1.compute.internal    Ready    master         86m   v1.22.0-rc.0+894a78b
ip-10-0-4-99.eu-central-1.compute.internal    Ready    master         87m   v1.22.0-rc.0+894a78b
```

## Deploying NetApp Astra Trident in ROSA

The first step is download and extract Astra Trident installer:

````
[ec2-user@ip-10-0-1-145 ~]$ wget wget https://github.com/NetApp/trident/releases/download/v21.07.2/trident-installer-21.07.2.tar.gz

[ec2-user@ip-10-0-1-145 ~]$ tar xvf trident-installer-21.07.2.tar.gz 
````

The best practice is deploy Astra Trident in his own project in ROSA. In order to do that:

`````
[ec2-user@ip-10-0-1-145 ~]$ ./oc new-project trident --display-name "Trident"
Now using project "trident" on server "https://api.pcsemeaos.xxx.xx.openshiftapps.com:6443".
`````
Before go with Astra Trident installation itself, kubectl needs to be in place because Astra Trident is completely integrated in k8s cluster so we need the service under the hood as executable service:

````
[ec2-user@ip-10-0-1-145 trident-installer]$ sudo yum update
[ec2-user@ip-10-0-1-145 trident-installer]$ sudo yum install -y kubectl
[ec2-user@ip-10-0-1-145 trident-installer]$ chmod +x ./kubectl
[ec2-user@ip-10-0-1-145 trident-installer]$ sudo mv ./kubectl /usr/local/bin/kubectl
````

Let's install Astra Trident:

`````
[ec2-user@ip-10-0-1-145 trident-installer]$ ./tridentctl install -n trident
INFO Created Kubernetes clients.                   namespace=default version=v1.22.0-rc.0+894a78b
INFO Starting Trident installation.                namespace=trident
...
...
INFO Trident REST interface is up.                 version=21.07.2
INFO Trident installation succeeded.  
`````

Once installed, check the ROSA Console to determine Astra Trident DaemonSet and Pods in order to assure High-Availability of the deployment:

![Screen Shot 2021-10-26 at 11 49 44](https://user-images.githubusercontent.com/59535705/142594845-b9eee2c6-b0ff-4fd2-949b-828fa9e229bc.png)

![Screen Shot 2021-10-26 at 11 49 53](https://user-images.githubusercontent.com/59535705/142594870-f0907b35-ec9b-4cc3-85dd-89e83bacfa68.png)

Congrats! Astra Trident was deployed succesfully in ROSA cluster. See the next steps to configure AWS FSx for ONTAP as persistent storage backend:

````
[ec2-user@ip-10-0-1-145 trident-installer]$ ./tridentctl create backend --filename backend-ontap-nas.json -n trident
+---------------+----------------+--------------------------------------+--------+---------+
|     NAME      | STORAGE DRIVER |                 UUID                 | STATE  | VOLUMES |
+---------------+----------------+--------------------------------------+--------+---------+
| BackendForNAS | ontap-nas      | b0c3e754-21aa-4bd0-8930-21bc6cc7113c | online |       0 |
+---------------+----------------+--------------------------------------+--------+---------+
````

Ready! 

Now AWS FSx for ONTAP service is available as persistent stogare so the next step will be create StorageClasses and the first PVs [here.](/1-FSx_ONTAP_PVs/README-PVs.md)