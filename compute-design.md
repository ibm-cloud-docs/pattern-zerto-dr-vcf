---
copyright:
  years: 2024
lastupdated: "2024-03-29"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Compute design
{: #compute-design}

## Requirements
{: #compute-requirements}

The requirements for the compute aspect for the Zerto for disaster recovery for VMware workloads pattern focus on:

- The compute required for the Zerto components.
- The compute aspect for the recovered workloads.

### Architecture components
{: #architecture-components}

- ZVM:
   - A Linux based virtual appliance, one per site.
   - ZVM uses a local embedded SQL Server by default. It's recommended to use an external Microsoft SQL Server for medium and larger sized environments, for example, \>250 incoming VPGs, to prevent performance degradation. For more information, see [Database requirements](https://help.zerto.com/bundle/Install.VC.HTML/page/Database_Requirements.htm){: external}.
   - For sizing information, see [ZVM Appliance Requirements, Supported Features &amp; Configurations](https://help.zerto.com/bundle/Linux.ZVM.HTML.10.0_U3/page/Book_in_Portal_-_Prerequisite_for_ZVM_Linux.htm){: external}.
- VRA:
   - A Linux based virtual machine that is installed on every hypervisor hosting virtual machine that requires protecting in the protected site and every hypervisor hosting replicated virtual machine in the recovery site.
   - Install a VRA on every hypervisor host so that if protected, virtual machines are moved from one host to another host in the cluster to ensure that there is a VRA to protect the moved virtual machines.
   - A VRA compresses the data that is passed across the network from the protected site to the recovery site. The VRA automatically adjusts the compression level according to CPU usage, including totally disabling it if needed.
   - A VRA can manage a maximum of 1500 volumes, whether these volumes are being protected or recovered.
   - For sizing information, see [Requirements for Virtual Replication Appliances](https://help.zerto.com/bundle/Prereq.VC.HTML.90/page/Requirements_for_Virtual_Replication_Appliances.htm){: external}.

## Deployment options
{: #deployment-options}

Two deployment options for VMware Cloud Foundation on {{site.data.keyword.vpc_short}} are available. These deployment options determine where the Zerto components need to be deployed.

* Standard architecture: Separate VMware VCF management and workload domains.
* Consolidated architecture: A consolidated domain where both the VCF management and workload domains reside.

### Standard architecture deployment on VCF
{: #vcfdeployment}

The following diagram introduces the high-level steps to deploy Zerto on a VMware Cloud Foundation standard architecture. In this architecture pattern, the Zerto ZVM appliance is deployed into the management domain as virtual machines (VMs) and Zerto VRAs are deployed in the VI workload domain.

![Zerto_VCF_IBM_Cloud_Standard_Architecture](image/Zerto-Architecture-Standard.svg){: caption="Figure 1. Zerto Disaster Recovery solution for VMware Workloads on {{site.data.keyword.vpc_short}} standard architecture" caption-side="bottom"}

Review the following steps to understand the standard architecture pattern deployment:

1. Create a bare metal server VLAN interface into management subnet for Zerto ZVM. Attach to equivalent management security groups. Add required DNS A and PTR records to the DNS service by following the Zerto documentation and your solution requirements.
2. Deploy Zerto ZVM into the VMware VM and attach it to the management Distributed Port Group (DPG) by using the provisioned IP address. Plan and size your deployment. Obtain a license through the VMware Solutions console.
3. Create the required number of bare metal server VLAN interfaces with reserved IP addresses by using consecutive IP range into management subnet for Zerto VRAs. Attach to equivalent management security groups in Virtual Private Cloud (VPC).
4. Configure Zerto ZVM and deploy VRAs by using the IP addresses attached to the VLAN interfaces.

### Consolidated architecture deployment on VCF
{: #compute-architecture}

The following diagram introduces the high-level steps to deploy Zerto on a VMware Cloud Foundation consolidated architecture. In this architecture pattern, Zerto appliances are deployed into the management domain as virtual machines (VMs).

![Zerto_VCF_IBM_Cloud_Consolidated_Architecture](image/Zerto-Architecture-Consolidated.svg){: caption="Figure 2. Zerto Disaster Recovery solution for VMware Workloads on {{site.data.keyword.vpc_short}} consolidated architecture" caption-side="bottom"}

Review the following steps to understand the consolidated architecture pattern deployment:

1. Create a bare metal server VLAN interface into management subnet for Zerto ZVM. Attach to equivalent management security groups. Add required DNS A and PTR records to the DNS service according to the Zerto documentation and your solution requirements.
2. Deploy Zerto ZVM into VMware VM and attach it to the management Distributed Port Group (DPG) by using the provisioned IP address. Plan and size your deployment. Obtain a license through the VMware Solutions console.
3. Create the required number of bare metal server VLAN interfaces with reserved IP addresses by using consecutive IP range into management subnet for Zerto VRAs. Attach to equivalent management security groups in Virtual Private Cloud (VPC).
4. Configure Zerto ZVM and deploy VRAs by using the IP addresses attached to the VLAN interfaces.

### Disaster recovery site compute sizing
{: #disaster-recovery-compute-sizing}

Ensure adequate bare metal ESXi hosts are provisioned in the recovery location to support replicas of critical workloads if there is a disaster.

Minimize costs by running sacrificial development and test workloads in the recovery site, which can be powered off during a disaster recovery event. Offloading workloads to the recovery site also reduces ESXi host quantity or size on the production site.
