# Using AWS FSx for NetApp ONTAP for OpenShift workloads
Succesfully use AWS FSx for NetApp ONTAP as persistent storage for your OpenShift workloads.

## Introduction

At the begining of the times, we used to work with stateless applications running in containers. Now, we find containers in the critical business applications so they need to use persistent storage to keep the business up and running.

NetApp is a CNCF co-funder member and this implies the maturity of NetApp solutions offering persistent storage for containers. In this use case it's deployed AWS FSx for ONTAP solution giving containers a fully ONTAP data management features.

## Architecture

- In this scenario I'm deploying the following services and components for the complete solution:
    - AWS FSx for ONTAP deployed in Frankfurt region using two AZs and one private subnet on each.
        - One of the private subnets will be the prefered for accessing the data and the another one is the stand-by subnet.
    - AWS ROSA single-AZ deployment in the same region and subnets to provide direct connection to AWS FSx for ONTAP.
        - A public subnet and outbound connection is necessary in order to manage AWS ROSA cluster from Red Hat console. 
        - An IDP authentication method in order to manage AWS ROSA cluster from Red Hat console.

Reference high-level view:

<img width="1136" alt="Screen Shot 2021-11-08 at 14 59 49" src="https://user-images.githubusercontent.com/59535705/140754953-4521d6eb-1de2-4179-9c92-377a5408e272.png">

