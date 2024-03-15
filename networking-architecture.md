---
copyright:
  years: 2024
lastupdated: "2024-03-14"

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
| Cross-region connectivity                                                       | Provide connectivity between the two IBM regions for the replication component | - Global Transit Gateway  \n -  Site to Site VPN                                                                | Global Transit Gateway                  | Connecting over VPC IBM core network, allows better performance without unnecessary IPsec overhead (Zerto components can natively use TLS to communicate with each other) |
| Public Access                                                                   | Internet access to connect to Zerto Call Home Server and Zerto Analytics.      | - Public gateway VPC subnet functionality  \n -  Proxy server or third party router  \n -  public IP assigned to the ZVM | Public gateway VPC subnet functionality | Built-in {{site.data.keyword.vpc_short}} functionality, providing some level of security (NAT is used with no direct internet exposure of the ZVM)                                    |
{: caption="Table 1. Architecture decisions for network" caption-side="bottom"}
