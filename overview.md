---
copyright:
  years: 2024
lastupdated: "2024-03-14"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Overview
{: #overview}

This pattern describes a Zerto deployment for disaster recovery for VMware Cloud Foundation (VCF) workloads where both the protected and recovery sites are hosted in {{site.data.keyword.vpc_full}} (VPC).

**While the provisioning of the VCF in {{site.data.keyword.vpc_short}} is automated, the installation and configuration of Zerto is a manual process and a custom installation on {{site.data.keyword.cloud_notm}}.**

This pattern is cross-region, meaning that the recovery site is in a different {{site.data.keyword.cloud_notm}} region than the protected location, for example protected site is Frankfurt and the recovery location is Madrid. However, if required the pattern can use a recovery site in the same geographic region, but a different Availability Zone if required, for example Frankfurt AZ1 and Frankfurt AZ3.

**This pattern does not cover backup design and deployment.**{: note}

Check to ensure that the minimum distance between the protected and recovery sites meets your requirement. {: important}

The following decision tree illustrates the selection criteria for selecting Zerto as the disaster recover product.

![Disaster_recovery_for_VMware workloads_on_ibm_cloud_decision_tree](image/Zerto-tree.svg){: caption="Disaster_recovery_for_VMware workloads_on_ibm_cloud_decision_tree" caption-side="bottom"}

Figure 1. Disaster recovery decision tree for {{site.data.keyword.vmwaresolutions_full_notm}}

Following the Architecture Framework, the `pattern-zerto-dr-vcf` covers design considerations and architecture decisions for the following aspects and domains:

- Compute: Virtual Servers
- Storage: Primary Storage, Backup Storage
- Networking: Enterprise Connectivity, Segmentation, and Isolation, Cloud Native Connectivity, Load Balancing, DNS
- Security: Data Security, Identity and Access Management, Application Security, Infrastructure and Endpoint Security
- Resiliency: High Availability, Backup, and Restore
- Service Management: Monitoring, Logging, Auditing, Alerting

 The Architecture Framework offers a consistent approach to designing cloud solutions by addressing requirements across various technology-agnostic architectural areas, called "aspects" and "domains." For more details, see [Introduction to the Architecture Framework](/docs/architecture-framework). The Architecture Framework offers a consistent approach to designing cloud solutions by addressing requirements across various technology-agnostic architectural areas, called "aspects" and "domains."
