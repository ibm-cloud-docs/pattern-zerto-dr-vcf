---
copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for storage

{: \#storage-decisions}

## Storage Architecture decision

| **Architecture decision**                                                 | **Requirement**                        | **Options** | **Decision** | **Rationale**                                                              |
| ------------------------------------------------------------------------------- | -------------------------------------------- | ----------------- | ------------------ | -------------------------------------------------------------------------------- |
| ZVM Storage                                                                     | Provide storage for the Zerto ZVM component. | VMware vSAN       | VMware vSAN        | vSAN is the only storage supported for datastores in VMware VCF on IBM Cloud VPC |
| VRA Storage                                                                     | Provide storage for the Zerto VRA component. | VMware vSAN       | VMware vSAN        | vSAN is the only storage supported for datastores in VMware VCF on IBM Cloud VPC |
| {: caption="Table 1. Architecture decisions for storage" caption-side="bottom"} |                                              |                   |                    |                                                                                  |
