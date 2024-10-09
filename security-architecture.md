---
copyright:
  years: 2024
lastupdated: "2024-03-14"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Architecture Decisions for Security
{: #security-architecture}

## Security Architecture decision
{: #ad-security}

| Architecture decision                                                  | Requirement                                                                               | Options                                                                       | Decision                     | Rationale                                                                                                                                                                                                                        |
| -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Encryption at rest                                                               | Encrypt all protected VM data                                                                      |  VM file encryption                                                                      | VM file encryption                     | File encryption on a VM with applications such as BitLocker for non-boot disks is supported by Zerto                                                                                                                                       |
| Data encryption and privacy in transit                                               | Encrypt and protect the protected VMs data in transit as it is replicated between the {{site.data.keyword.IBM_notm}} regions | - Enable native TLS-based VRA encryption  \n -  No encryption  \n -  connection that is considered as private | Enable native TLS-based VRA encryption | Native Zerto capability to protect sensitive replication data in-flight. For more information, see [VRA to VRA Encryption](https://help.zerto.com/bundle/Security.Hardening.HTML/page/Virtual_Replication_Appliance.htm#vra_to_vra_encryption) |
{: caption="Architecture decisions for security" caption-side="bottom"}
