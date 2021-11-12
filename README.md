# Using AWS FSx for NetApp ONTAP for Red Hat OpenShift Service on AWS (ROSA) workloads
Succesfully use AWS FSx for NetApp ONTAP as persistent storage for your OpenShift workloads.

## Introduction

At the begining of the times, we used to work with stateless applications running in containers. Now, we find containers in the critical business applications so they need to use persistent storage to keep the business up and running.

NetApp is a CNCF co-funder member and this implies the maturity of NetApp solutions offering persistent storage for containers. In this use case it's deployed AWS FSx for ONTAP solution giving containers a fully ONTAP data management features.

## Architecture

- In this scenario I'm deploying the following services and components for the complete solution:
    - AWS FSx for ONTAP deployed in Frankfurt region using two AZs and one private subnet on each.
        - One of the private subnets will be the prefered for accessing the data and the another one is the stand-by subnet.
    - ROSA single-AZ deployment in the same region and subnets to provide direct connection to AWS FSx for ONTAP.
        - A public subnet and outbound connection is necessary in order to manage ROSA cluster from Red Hat console. 
        - An IDP authentication method in order to manage AWS ROSA cluster from Red Hat console.
        - NetApp Astra Trident as persistent storage provisioner deployed inside ROSA cluster.

### Reference high-level view

<img width="928" alt="Screen Shot 2021-11-08 at 15 10 43" src="https://user-images.githubusercontent.com/59535705/140756731-aa70cbad-0cc4-462d-b0b0-924c1bfe7321.png">

## Setup and testing

My recommendation is going around AWS docs to deploy AWS FSx for ONTAP service so you will understand all the concepts before going steps further -> [AWS FSx for ONTAP docs](https://docs.aws.amazon.com/fsx/latest/ONTAPGuide/getting-started.html)

For better understanding about how ROSA works take a look around [ROSA docs](https://docs.aws.amazon.com/ROSA/latest/userguide/getting-started.html)

And finally go around Trident resources before deploying it so you will understand all the concepts before going steps further -> [Trident docs](https://github.com/NetApp/trident)

Click [here](0-Initial_setup/README-initial.md) to follow the steps in order to do the initial setup and deploy Trident inside ROSA and next steps.
