---
copyright:
  years: 2024
lastupdated: "2024-03-14"

subcollection: pattern-zerto-dr-vcf

keywords:
---
# Architecture decisions for resiliency
{: #ad-resiliency}

The following sections summarize the resiliency architecture decisions for workloads deployed on {{site.data.keyword.cloud_notm}} VCF for Zerto.

| Architecture decision                                                     | Requirement                                                                         | Options                                         | Decision | Rationale                                                                                                                                                                                                                 |
| ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- | --------------------------------------------------------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Replication type between the {{site.data.keyword.cloud_notm}} regions                                      | Replicate the VM data between the IBM regions so that the VMs might be recovered.             |  Zerto CDP                                                 | Zerto CDP          | Recovery point objective (RPO) in seconds with crash-consistent application checkpoints enabling full application recovery with a minimal loss of data                                                                                                     |
| Disaster recovery solution resiliency                                               | Provide a way to recover the functions of Zerto in case it becomes unavailable | - vSphere HA  \n -  ZVMs deployed on a Microsoft Failover Cluster | vSphere high availability (HA)         | Local native vSphere HA does not require any specific configuration. ZVMs sync, which allows for recovery of the protected VMs even if the protected site is lost. |
{: caption="Table 1. Architecture decisions for resiliency" caption-side="bottom"}
