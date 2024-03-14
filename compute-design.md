---
copyright:
  years: 2024
lastupdated: "2024-03-14"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Compute design

{: \#compute-design}

This section expands on the compute aspect of the IBM Architecture framework in respect of the Zerto disaster recovery for VMware Cloud Foundation in IBM Cloud VPC pattern.

## Requirements

The requirements for the compute aspect for the Zerto for disaster recovery for VMware workloads pattern focus on the following:

- The compute required for the Zerto components.
- The compute aspect for the recovered workloads.

Zerto Architecture Components:

- **ZVM:**
  - A Linux based virtual appliance, one per site.
  - Uses a local embedded SQL Server by default, it is recommended to use an external Microsoft SQL Server for medium sized and larger environments (\>250 incoming VPGs) to prevent performance degradation. https://help.zerto.com/bundle/Install.VC.HTML/page/Database_Requirements.htm
  - For sizing see [ZVM Appliance Requirements, Supported Features &amp; Configurations](https://help.zerto.com/bundle/Linux.ZVM.HTML.10.0_U3/page/Book_in_Portal_-_Prerequisite_for_ZVM_Linux.htm)
- **VRA:**
  - A Linux based virtual machine installed on every hypervisor that hosts virtual machines that require protecting in the **protected** site and on every hypervisor that will host the replicated virtual machines in the **recovery site**. Install a VRA on every hypervisor host so that if protected virtual machines are moved from one host in the cluster to another host in the cluster there is always a VRA to protect the moved virtual machines.
  - A VRA compresses the data that is passed across the network from the protected site to the recovery site. The VRA automatically adjusts the compression level according to CPU usage, including totally disabling it if needed.
  - A VRA can manage a maximum of 1500 volumes, whether these volumes are being protected or recovered.
  - For sizing, see [Requirements for Virtual Replication Appliances](https://help.zerto.com/bundle/Prereq.VC.HTML.90/page/Requirements_for_Virtual_Replication_Appliances.htm).

## Deployment Options

There are two deployment options for VMware Cloud Foundation on IBM Cloud VPC. These deployment options will determine where the zerto components need to be deployed.

1. **Standard Architecture:** Separate VMware VCF Management and Workload domains.
2. **Consolidated Architecture:** Consolidated domain where both the VCF Management and Workload domains reside.

### Standard architecture deployment on VCF

The following diagram introduces the high-level steps to deploy Zerto on a VMware Cloud Foundation standard architecture. In this architecture pattern, the Zerto ZVM appliance is deployed into the management domain as virtual machines (VMs) and Zerto VRAs are deployed in the VI workload domain.

![Zerto_VCF_IBM_Cloud_Standard_Architecture](image/Zerto-Architecture-Standard.svg)

Figure 1. Zerto Disaster Recovery solution for VMware Workloads on IBM Cloud VPC standard architecture

This architecture pattern deployment is summarized as follows:

1. Create a bare metal server VLAN interface into management subnet for Zerto ZVM. Attach to equivalent management security groups. Add required DNS A and PTR records to the DNS service by following the Zerto documentation and your solution requirements.
2. Deploy Zerto ZVM into the VMware® VM and attach it to the management DPG (distributed port group) by using the provisioned IP address. Plan and size your deployment. Obtain a license through the VMware Solutions console.
3. Create the required number of bare metal server VLAN interfaces with reserved IP addresses by using consecutive IP range into management subnet for Zerto VRAs. Attach to equivalent management security groups in Virtual Private Cloud (VPC).
4. Configure Zerto ZVM and deploy VRAs by using the IP addresses attached to the VLAN interfaces.

### Consolidated architecture deployment on VCF

The following diagram introduces the high-level steps to deploy Zerto on a VMware Cloud Foundation consolidated architecture. In this architecture pattern, Zerto appliances are deployed into the management domain as virtual machines (VMs).

![Zerto_VCF_IBM_Cloud_Consolidated_Architecture](image/Zerto-Architecture-Consolidated.svg)

Figure 2. Zerto Disaster Recovery solution for VMware Workloads on IBM Cloud VPC consolidated architecture

This architecture pattern deployment is summarized as follows:

1. Create a bare metal server VLAN interface into management subnet for Zerto ZVM. Attach to equivalent management security groups. Add required DNS A and PTR records to the DNS service according to the Zerto documentation and your solution requirements.
2. Deploy Zerto ZVM into VMware® VM and attach it to the management DPG (distributed port group) by using the provisioned IP address. Plan and size your deployment. Obtain a license through the VMware Solutions console.
3. Create the required number of bare metal server VLAN interfaces with reserved IP addresses by using consecutive IP range into management subnet for Zerto VRAs. Attach to equivalent management security groups in Virtual Private Cloud (VPC).
4. Configure Zerto ZVM and deploy VRAs by using the IP addresses attached to the VLAN interfaces.

### Disaster recovery site compute sizing

The disaster recovery environment needs to always have enough bare metals ESXi hosts provisioned in the recovery location to be able to bring up the replicas of the protected workloads when a disaster renders the production VMware environment unavailable.

To optimize the cost of having bare metal ESXi hosts constantly provisioned on the recovery site with no actual workload, a possibility is to run “sacrificial” development or test workloads, for which no disaster recovery is needed and that could be powered off to free capacity in the event of a disaster recovery invocation or test, on the recovery site. Offloading this type of workloads to the recovery site also reduces the number/size of ESXi hosts needed on the production site.
