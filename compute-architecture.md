---
copyright:
  years: 2024
lastupdated: "2024-03-29"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

## Compute Architecture decision
{: #ad-compute}

| Architecture decision                                            | Requirement                  | Options                            | Decision                  | Rationale                                                                                                                                                                                                                                                                                                     |
| -------------------------------------------------------------------------- | -------------------------------------- | -------------------------------------------- | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Zerto ZVM deployment type                                                  | Provide deployment type for Zerto ZVM. | - VM hosted on the VMware environment  \n -  VPC VSI | VM hosted on the VMware environment | Uses the Linux based appliance as recommended by Zerto. This appliance is not suitable for a VPC VSI but can be deployed as a VM hosted on the VMware environment. No Windows license needed, simplified deployment (OVF template), reduced footprint, increased security (no possibility to directly modify the host OS) |
{: caption=" Table 1 compute architecture decision" caption-side="bottom"}

## Database Architecture decision
{: #ad-database}

| Architecture decision                                             | Requirement                                    | Options                                        | Decision      | Rationale                                                                                                                                                                                                                                                                     |
| --------------------------------------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------- | ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Database                                                                    | Provide a performant solution for large-scale environments | - Zerto embedded database  \n - Microsoft SQL Server | Zerto embedded database | Dependent on the environment size, Zerto's recommendation is to use the embedded database. For larger environments (\>250 incoming VPGs), use an external Microsoft SQL Server. For more information, see [sizing considerations](https://help.zerto.com/bundle/Install.VC.HTML/page/Database_Requirements.htm). |
{: caption=" Table 2 Database architecture decision" caption-side="bottom"}
