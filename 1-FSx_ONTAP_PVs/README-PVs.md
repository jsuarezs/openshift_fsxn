## Create StorageClasses and PVCs/PVs

In order to deploy the first volume we need to create the NAS and SAN StorageClasses pointing to AWS FSx for ONTAP backend:

````
[ec2-user@ip-10-0-1-145 ~]$ ./oc apply -f trident-installer/sc-nas.yaml 
storageclass.storage.k8s.io/nas created
[ec2-user@ip-10-0-1-145 ~]$ 
[ec2-user@ip-10-0-1-145 ~]$ ./oc get sc
NAME            PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
gp2 (default)   kubernetes.io/aws-ebs   Delete          WaitForFirstConsumer   true                   138m
gp2-csi         ebs.csi.aws.com         Delete          WaitForFirstConsumer   true                   138m
nas             csi.trident.netapp.io   Retain          Immediate              true                   8s
````

This is something can be done by ROSA GUI indeed:


![Screen Shot 2021-10-26 at 12 04 42](https://user-images.githubusercontent.com/59535705/142596746-5e2c655c-e760-43ed-b528-6b19652b9607.png)

The same procedure applies to sc-san.yaml StorageClasses to take benefits from AWS FSx for ONTAP multiprotocol access.

Now it's time to create the first PVC/PV:

````
[ec2-user@ip-10-0-1-145 ~]$ ./oc apply -f vol-nas.yaml 
persistentvolumeclaim/vol1 created

[ec2-user@ip-10-0-1-145 ~]$ ./oc get pvc
NAME   STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
vol1   Bound    pvc-70c135e4-7597-4485-b692-8b8f313e61be   2Gi        RWX            nas            8s
`````

Now we can check that our PVC bound to a new PV in AWS FSx for ONTAP providing persistn storage for pods. Check the new volume in AWS console and RedHat Console:

![Screen Shot 2021-10-26 at 12 24 29](https://user-images.githubusercontent.com/59535705/142597125-062516ac-227d-4150-bf62-ce0706ce08fc.png)

![Screen Shot 2021-10-26 at 12 14 36](https://user-images.githubusercontent.com/59535705/142597150-6f14a32c-b724-49e3-af36-d9150372b501.png)

Now play around the different volume access RWO/ROX/RWX modes are available:

- Read Write Once (RWO) – only one node is allowed to access the storage volume at a time for read and write access.
- Read Only Many (ROX) – many nodes may access the storage volume in read-only mode.
- Read Write Many (RWX) – many nodes may simultaneously read and write to the storage volume.

![Screen Shot 2021-10-28 at 11 04 42](https://user-images.githubusercontent.com/59535705/142598685-524535b6-44a2-4343-ab0c-def6583f44c0.png)

Now we can use AWS FSx for ONTAP service as persistent stogare so the next step will be create StorageClasses and the first PVs [here.](/1-FSx_ONTAP_PVs/README-PVs.md)

Now we can deploy applications taking advantage of AWS FSx for ONTAP as persistent storage and get the ONTAP benefits on AWS platform.

To play around with NetApp efficient Snapshots let's go to the next section [here.](/2-CSI_Snapshots/README-CSI.md)