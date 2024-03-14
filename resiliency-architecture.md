---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-zerto-dr-vcf

keywords:
---

# Architecture decisions for resiliency

{: \#resiliency-architecture}

The following sections summarize the resiliency architecture decisions for workloads deployed on IBM Cloud VCF for Zerto.

| **Architecture decision**                      | **Requirement**                                                                               | **Options**                                               | **Decision** | **Rationale**                                                                                                                                                                                                                       |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|-----------------------------------------------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Replication type between the IBM Cloud regions | Replicate the VM data between the IBM regions so that the VMs could be recovered.             | Zerto CDP                                                 | Zerto CDP    | RPO in seconds with crash consistent application checkpoints allowing for full application recovery with a minimal loss of data                                                                                                     |
| Disaster recovery solution resiliency          | Provide a way to recover the functions of the Zerto components in case it becomes unavailable | vSphere HA. ZVMs deployed on a Microsoft Failover Cluster | vSphere HA   | Local native vSphere HA does not require any specific configuration, ZVMs in each site kept in sync allow to recover the protected VMs even if the protected site has been completely lost while keeping the RTO as low as possible |
