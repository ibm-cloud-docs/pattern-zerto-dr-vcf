---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-zerto-dr-vcf

keywords:
---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking}

## Network Architecture decision
{: #ad-network}

| **Architecture decision**                                                       | **Requirement**                                                                | **Options**                                                                                             | **Decision**                            | **Rationale**                                                                                                                                                             |
|---------------------------------------------------------------------------------|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cross-region connectivity                                                       | Provide connectivity between the two IBM regions for the replication component | Global Transit Gateway. Site to Site VPN                                                                | Global Transit Gateway                  | Connecting over VPC IBM core network, allows better performance without unnecessary IPSec overhead (Zerto components can natively use TLS to communicate with each other) |
| Public Access                                                                   | Internet access to connect to Zerto Call Home Server and Zerto Analytics.      | Public gateway VPC subnet functionality, Proxy server/third party router, public IP assigned to the ZVM | Public gateway VPC subnet functionality | Built-in {{site.data.keyword.vpc_short}} functionality, providing some level of security (NAT is being used with no direct internet exposure of the ZVM)                                    |
{: caption="Table 1. Architecture decisions for network" caption-side="bottom"}
