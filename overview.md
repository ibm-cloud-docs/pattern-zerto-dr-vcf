---
copyright:
  years: 2024
lastupdated: "2024-03-26"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Overview
{: #overview}

The Zerto deployment for disaster recovery for VMware Cloud Foundation (VCF) workloads pattern parovides a solution design where both the protected and recovery sites are hosted in {{site.data.keyword.vpc_full}}. 

While the provisioning of the VCF in {{site.data.keyword.vpc_short}} is automated, the installation and configuration of Zerto is a manual process and a custom installation on {{site.data.keyword.cloud_notm}}.
{: note}

This pattern is cross-region, the recovery site is in a different {{site.data.keyword.cloud_notm}} region than the protected location. For example, let's say the protected site is Frankfurt and the recovery location is Madrid. However, if required, the pattern can use a recovery site in the same geographic region, but a different availability zone such as Frankfurt AZ1 and Frankfurt AZ3.

Check to ensure that the minimum distance between the protected and recovery sites meets your requirement. {: important}

The following decision tree illustrates the selection criteria for selecting Zerto as the disaster recover product. This pattern does not cover backup design and deployment.

![Disaster_recovery_for_VMware workloads_on_ibm_cloud_decision_tree](image/Zerto-tree.svg){: caption="Disaster_recovery_for_VMware workloads_on_ibm_cloud_decision_tree" caption-side="bottom"}

Figure 1. Disaster recovery decision tree for {{site.data.keyword.vmwaresolutions_full_notm}}
