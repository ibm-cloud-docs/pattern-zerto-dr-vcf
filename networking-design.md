---
copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Network design
{: #network-design}

This section expands on the network aspect of the IBM Architecture framework in respect of the Zerto disaster recovery for VMware Cloud Foundation in {{site.data.keyword.vpc_short}} pattern.

## Requirements
{: #requirements}

The requirements for the network aspect for the Zerto for disaster recovery for VMware workloads pattern focus on the following:

- Replicate the VMware workloads in accordance with the target RPO.
- Internet connectivity – Each ZVM requires internet access, either directly or indirectly through a proxy, for billing purposes. If you do not complete the configuration, Zerto blocks management activities in 15 days. Internet access also used to enable site monitoring using the Zerto Analytics and Zerto Mobile App platforms.
- The ZVM in the protected site must be able to communicate with the ZVM in the recovery site.
- The ZVMs from each site must be able to communicate with the local ESXi hosts and vCenter.
- The VRAs from each site must be able to communicate with the local ZVM.
- The VRAs in the protected site must be able to communicate with the VRAs in the recovery site for the replication and restoration of workloads.
- Cross VPC traffic flow. In this pattern, the connectivity between the protected and recovery site is achieved by using a global transit gateway. The transit gateway is an {{site.data.keyword.cloud_notm}} component allowing to easily interconnect {{site.data.keyword.vpc_short}}s (as well as {{site.data.keyword.vpc_short}} classic and {{site.data.keyword.powerSys_notm}} environments). In the case of a global transit gateway, these resources can be located in different {{site.data.keyword.vpc_short}} regions.

When you design or deploy this architecture pattern, consider the following:

- Zerto architecture does not support NAT (Network Address Translation) firewalls.
- Design your network flows carefully. For more information about ports and protocols, see [Zerto network ports and protocols](https://help.zerto.com/bundle/Admin.VC.HTML.90/page/Port_Usage.htm).
- Ensure that the {{site.data.keyword.vpc_short}} security groups and firewall rules allow the replication and control traffic.
- Ensure that the networks are properly routed and possible firewall rules allow the required traffic at both source and destination sites.
- Performance:
   - Amount of initial data initialized or synchronized in a VPG.
   - Rate of change of data/RPO - sizes of changed data and data change rate.
- Transit Gateway traffic cost implications.

## Considerations
{: #considerations}

Here are some key areas that you must keep in mind while designing Zerto replication for Disaster recovery environment on {{site.data.keyword.cloud_notm}}.

| **Area**                                                                                                                                                                                      | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Firewall Configuration:**                                                                                                                                                                   | Ports and Protocols: Ensure that the necessary ports and protocols are open in the firewalls between Zerto components, VMware infrastructure, and any other relevant systems. Zerto provides documentation specifying the required ports for different components and functionalities. Ensure that the required firewall ports are open between the recovered VMs (e.g.: NSX-T distributed firewall rules) and their eventual external dependencies. |
| **Public Access**                                                                                                                                                                       |Internet Access to connect to Zerto CallHome Server, Zerto support, Zerto Analytics. Proxy or NAT connection, for example by using the VPC subnet public gateway. |                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| **Network Segmentation:**                                                                                                                                                                     | **Isolation**: Segment the network to isolate the Zerto components from other critical infrastructure. This can enhance security and prevent unauthorized access.                                                                                                                                                                                                                                                                              |
| **VMware vSphere Connectivity:**                                                                                                                                                              | **vCenter and ESXi Host Connectivity:** Establish reliable connections to VMware vCenter and ESXi hosts to facilitate the discovery of VMs, configuration settings retrieval, and other interactions between Zerto and VMware.                                                                                                                                                                                                                 |
| **Secure Communication:**                                                                                                                                                                     | **TLS/SSL Encryption:** Enable TLS/SSL encryption for communication between Zerto components to ensure the confidentiality and integrity of data in transit. **Secure Communication with VMware:** Use secure communication protocols when interacting with VMware infrastructure, adhering to best practices for securing VMware environments.                                                                                          |
| **DNS Configuration:**                                                                                                                                                                        | **Name Resolution:** Ensure proper DNS configuration for Zerto components and VMware infrastructure to enable seamless name resolution. This is essential for the identification and communication between components. Update the DNS records to point to the recovered VMs.                                                                                                                                                                   |
| **Time Synchronization:**                                                                                                                                                                     | **NTP (Network Time Protocol):** Synchronize the clocks across Zerto components, VMware hosts, and other relevant systems using NTP to ensure accurate timestamps for logs and data consistency.                                                                                                                                                                                                                                               |
| **Redundancy and High Availability:**                                                                                                                                                         | **Redundant Network Paths:** Configure redundant network paths to provide high availability and fault tolerance, ensuring that a failure in one path does not disrupt data transfer operations.                                                                                                                                                                                                                                                |
{: caption="Table 1. Networking design considerations" caption-side="bottom"}
