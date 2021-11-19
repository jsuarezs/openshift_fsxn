## Create Snapshots from PVCs

NetApp Snapshots are well know for their efficient instantaneous and efficient capability.

Those instant copies are not taking space until the source data start to change. Thanks to only pointers to the original blocks are created they are instantaneous and don't use the same source space at creation time.

This enables a faster DevOps/Test flow for cloning purposes.

This process will be done by GUI so the first step is create VolumeSnapshotClass. Once created it's shown in VolumeSnapshotClasses section:

![Screen Shot 2021-10-28 at 15 23 56](https://user-images.githubusercontent.com/59535705/142601424-eb47698c-57bc-46cf-8a77-5061734feaee.png)

By CLI the procedure is another simple way to do it:

````
[ec2-user@ip-10-0-1-145 ~]$ ./oc apply -f snapshotclass.yaml 
````

Now let's create our first Snapshot from the PVC vol00 simulating a live application on ROSA by RedHat Console selecting csi-snapclass created before in the project application:

![Screen Shot 2021-10-28 at 15 25 39](https://user-images.githubusercontent.com/59535705/142601984-aefcfd16-1c79-476b-b30f-c115b100b13e.png)

Simple like this we already got a NetApp efficient Snapshot from vol00 that is available for restoring capabilities and even for cloning purposes. Let's use this new Snapshot to create a new volume from it:

![Screen Shot 2021-10-28 at 15 30 48](https://user-images.githubusercontent.com/59535705/142602599-9d9b2b52-f9c6-4857-b999-fd1382e2a101.png)

So whith this process it's easy and fast clone persistent volumes for DevOps/testing flows.

Now let's finish this guide showing how you could import your own volumes in AWS FSx for ONTAP using Astra Trident [here.](/3-Trident_Import/README-import.md)