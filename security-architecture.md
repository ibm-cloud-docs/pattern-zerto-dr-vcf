---
copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Security design

{: #security-architecture}

The following sections summarize the security architecture decisions for workloads deployed on {{site.data.keyword.cloud_notm}} VCF for Zerto.

## Security Architecture decision

| **Architecture decision**                                                  | **Requirement**                                                                               | **Options**                                                                       | **Decision**                     | **Rationale**                                                                                                                                                                                                                        |
| -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Encryption at rest                                                               | Encrypt all protected VMs data                                                                      | VM file encryption                                                                      | VM file encryption                     | File encryption on a VM with applications such as BitLocker for non-boot disks is supported by Zerto                                                                                                                                       |
| Data encryption/privacy in transit                                               | Encrypt/protect the protected VMs data in transit as it is replicated between the IBM Cloud regions | Enable native TLS-based VRA encryption, No encryption, connection considered as private | Enable native TLS-based VRA encryption | Native Zerto capability to protect sensitive replication data in-flight. For more information, see[VRA to VRA Encryption](https://help.zerto.com/bundle/Security.Hardening.HTML/page/Virtual_Replication_Appliance.htm#vra_to_vra_encryption) |
| {: caption="Table 1. Architecture decisions for security" caption-side="bottom"} |                                                                                                     |                                                                                         |                                        |                                                                                                                                                                                                                                            |
