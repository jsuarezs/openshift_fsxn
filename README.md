# Using AWS FSx for NetApp ONTAP for OpenShift workloads
Succesfully use AWS FSx for NetApp ONTAP as persistent storage for your OpenShift workloads.

## Introduction

At the begining of the times, we used to work with stateless applications running in containers. Now, we find containers in the critical business applications so they need to

## Architecture

- In this scenario I'm deploying the following services and components for the complete solution:
    - AWS FSx for ONTAP deployed in Frankfurt region using two AZs and one private subnet on each.
        - One of the private subnets will be the prefered for accessing the data and the another one is the stand-by subnet.
    - AWS ROSA service deployment in the same region and subnets to provide direct connection to AWS FSx for ONTAP.
        - A public subnet and outbound connection is necessary in order to manage AWS ROSA cluster from Red Hat console. 
        - An IDP authentication method in order to manage AWS ROSA cluster from Red Hat console.

