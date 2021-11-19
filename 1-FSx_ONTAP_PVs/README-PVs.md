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

