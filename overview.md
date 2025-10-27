---
copyright:
  years: 2024
lastupdated: "2024-03-26"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Overview
{: #overview-id}

**End of Marketing**: As of 31 October 2025, new deployments of VMware Solutions offerings are no longer available for new customers. Existing customers can still use and expand their active VMware® workloads on {{site.data.keyword.cloud}}. For more information, see [End of Marketing for VMware on {{site.data.keyword.cloud_notm}}](/docs/vmwaresolutions?topic=vmwaresolutions-eos-vms).
{: note}

The Zerto deployment for disaster recovery for VMware Cloud Foundation (VCF) workloads pattern provides a solution design where both the protected and recovery sites are hosted in {{site.data.keyword.vpc_full}}.

While the provisioning of the VCF in {{site.data.keyword.vpc_short}} is automated, the installation and configuration of Zerto is a manual process [ZVM configuration details](https://help.zerto.com/bundle/Linux.ZVM.HTML.10.0_U3/page/Book_in_Portal_-_Prerequisite_for_ZVM_Linux.htm){: external} on {{site.data.keyword.cloud_notm}}.
{: note}

This pattern is cross-region, the recovery site is in a different {{site.data.keyword.cloud_notm}} region than the protected location. For example, let's say the protected site is Frankfurt and the recovery location is Madrid. However, if required, the pattern can use a recovery site in the same geographic region, but a different availability zone such as Frankfurt AZ1 and Frankfurt AZ3.  Please look in the reference architecture section to gain more clarity on a typical deployment architecture of Zerto on IBM Cloud VMware.

Check to ensure that the minimum distance between the protected and recovery sites meets your requirement. {: important}

The following decision tree illustrates the selection criteria for selecting Zerto as the disaster recover product. This pattern does not cover backup design and deployment.

![Disaster_recovery_for_VMware workloads_on_ibm_cloud_decision_tree](image/Zerto-tree.svg){: caption="Disaster recovery decision tree for {{site.data.keyword.vmwaresolutions_full_notm}}" caption-side="bottom"}


Follow the above decision tree while selecting a Disaster recovery solution offering for IBM Cloud VMware solutions.
