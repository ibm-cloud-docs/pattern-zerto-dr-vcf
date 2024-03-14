---
copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: <pattern-zerto-dr-vcf>

keywords:
---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute

{: \#compute-decisions}

## Compute Architecture decision

| **Architecture decision**                                                  | **Requirement**                        | **Options**                                  | **Decision**                        | **Rationale**                                                                                                                                                                                                                                                                                                           |
|----------------------------------------------------------------------------|----------------------------------------|----------------------------------------------|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zerto ZVM deployment type                                                  | Provide deployment type for Zerto ZVM. | VM hosted on the VMware environment. VPC VSI | VM hosted on the VMware environment | Uses the Linux based appliance as recommended by Zerto. This appliance is not suitable for a VPC VSI but can be deployed as a VM hosted on the VMware environment. No Windows license needed, simplified deployment (OVF template), lower footprint, increased security (no possibility to directly modify the host OS) |
| {: caption=" Table 1 compute architecture decision" caption-side="bottom"} |                                        |                                              |                                     |                                                                                                                                                                                                                                                                                                                         |

## Database Architecture decision

| **Architecture decision**                                                   | **Requirement**                                          | **Options**                                              | **Decision**            | **Rationale**                                                                                                                                                                                                                                                                              |
|-----------------------------------------------------------------------------|----------------------------------------------------------|----------------------------------------------------------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Database                                                                    | Provide performant solution for large scale environments | Zerto embedded database. Additional Microsoft SQL Server | Zerto embedded database | Dependent on the environment size, Zerto's recommendation is to use the embedded database. For larger environments (\>250 incoming VPGs), use an external Microsoft SQL Server. See [Sizing considerations](https://help.zerto.com/bundle/Install.VC.HTML/page/Database_Requirements.htm). |
| {: caption=" Table 2 Database architecture decision" caption-side="bottom"} |                                                          |                                                          |                         |                                                                                                                                                                                                                                                                                            |
