---
copyright:
  years: 2024
lastupdated: "2024-03-29"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for storage
{: #storage-decisions}

## Storage Architecture decisions
{: #storage-architecture-decisions}

| Architecture decision                                                 | Requirement                       | Options | Decision | Rationale                                                              |
| ------------------------------------------------------------------------------- | -------------------------------------------- | ----------------- | ------------------ | -------------------------------------------------------------------------------- |
| ZVM Storage                                                                     | Provide storage for the Zerto ZVM component. | VMware vSAN       | VMware vSAN        | vSAN is the only storage that is supported for datastores in VMware VCF on {{site.data.keyword.vpc_short}} |
| VRA storage                                                                     | Provide storage for the Zerto VRA component. | VMware vSAN       | VMware vSAN        | vSAN is the only storage that is supported for datastores in VMware VCF on {{site.data.keyword.vpc_short}} |
{: caption="Architecture decisions for storage" caption-side="bottom"}
