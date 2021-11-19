## Why Volume Import

There are several reasons which could apply looking for an Astra Trident import feature, for example:

- Migrate existing monolithic applications into containers – Volume Import allows you to test and migrate current applications into containers without losing your data.

- Volume migration – Migrate your data from one Kubernetes cluster to another.

- Kubernetes re-build – If the Kubernetes cluster has a catastrophe and needs to be rebuilt from scratch, don’t lose your data! Keep the data and use volume import to retrieve the data and bring into the new re-built cluster.

- Data Management disaster recovery – When using SnapMirror, volume import can be used to import volumes from the backup site into the Kubernetes cluster.

The workflow in a high level is once the source application is gracefully shoutdown and unmount the volume, the NetApp Snapmirror relationship could be stablished between another ONTAP system to AWS FSxfor ONTAP moving efficiently on-prem ONTAP data to AWS. 

<img width="496" alt="Screen Shot 2021-11-19 at 11 35 18" src="https://user-images.githubusercontent.com/59535705/142608815-c369127b-b1a9-4b3b-8c00-6e1d44676956.png">

Once the volume is in AWS FSx for ONTAP is possible to import it to ROSA cluster using Astra Trident Import feature.

````
./tridentctl import volume BackendForNAS trident_pvc_34a9dcc0_e4cd_4d81_9248_8ad5f5bfd19e -f vol-nas-cloud.yaml -n trident
````

Getting started with NetApp Astra Trident Import feature [Getting started.](https://netapp-trident.readthedocs.io/en/stable-v20.07/kubernetes/operations/tasks/volumes/import.html)