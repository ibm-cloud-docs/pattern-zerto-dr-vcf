---
copyright:
  years: 2024
lastupdated: "2024-03-29"

subcollection: pattern-zerto-dr-vcf
keywords:

authors:
  - name: Dwarkanath Rao, Anne McDermaid, Bertrand- David
---
{{site.data.keyword.attribute-definition-list}}

# Deploy resiliency design with Zerto on VMware
{: #resiliency-zerto}

The Zerto for disaster recovery for VMware Cloud Foundations (VCF) workloads deploys a manual solution where both the protected and recovery sites are in {{site.data.keyword.vpc_short}}.  This pattern builds on Zerto best practice guidance. For more information, see [Zerto Best Practices](https://help.zerto.com/category/Best_Practices){: external}.

- Disaster recovery is a shared responsibility of the client and supported by {{site.data.keyword.IBM}} or a Business Partner for build and Day 2 support services. If you do not use {{site.data.keyword.IBM_notm}} or a Business Partner for build and Day 2 support services, then you are responsible for disaster recovery.
- If the workload to be protected must support data encryption, GPDR, and other regulatory compliance, see [Deploy {{site.data.keyword.cloud_notm}} Resiliency Design with Veeam on VMware](https://cloud.ibm.com/docs/vmware-cross-region-dr?topic=vmware-cross-region-dr-overview).
- This pattern is cross-region. The recovery site is in a different {{site.data.keyword.cloud_notm}} region than the protected location. For example, let's say the protected site is Frankfurt and the recovery location is Madrid. However, if required, the pattern can use a recovery site in the same geographic region, but in a different availability zone such as Frankfurt AZ1 and Frankfurt AZ3.
- Check to ensure that the minimum distance between the protected and recovery sites meets your requirement.

VCF in {{site.data.keyword.vpc_short}} can be deployed as one of two architectures, standard and consolidated. This pattern is based on the consolidated architecture but includes considerations for the standard architecture.

## Zerto Architecture diagram
{: #resiliency-architecture}

The following diagram describes the high-level steps to deploy Zerto on a VMware Cloud Foundation (VCF) deployed with the consolidated architecture. In this pattern, Zerto appliances are deployed into the management domain as virtual machines (VMs). The deployment architecture pattern is summarized as follows:

1. Create a bare metal server VLAN interface into management subnet for Zerto Virtual Manager (ZVM) This step provides an IP address from the management subnet for use for the Zerto Virtual Manager. Attach to equivalent management security groups. Add required DNS (Domain Name System ) A and PTR records ( DNS pointer record )to the DNS service according to the Zerto documentation and your solution requirements.
2. Deploy a Zerto Virtual Manager appliance and attach it to the management Distributed Port Group by using the provisioned IP address. Plan and size your deployment. Obtain a license through the VMware Solutions console.
3. Create the required number of bare metal server VLAN interfaces with reserved IP addresses by using consecutive IP range into management subnet for Zerto VRAs ( Virtual Replication appliances ). Attach to equivalent management security groups in Virtual Private Cloud (VPC).
4. Configure Zerto Virtual Manager with the IP address range and use the ZVM ( Zerto Virtual Manager ) workflow to deploy the VRAs.

![img](image/Zerto-Architecture-Disaster-recovery.svg){: caption="Figure 1. Zerto Disaster Recovery for VMWare Solution Components" caption-side="bottom"}

In the following reference architecture, a consolidated VCF deployment in two regions is assumed. Review the key features of this pattern:

- **{{site.data.keyword.cloud_notm}} Infrastructure:**
    - Two VMware Cloud Foundation environments that are hosted on {{site.data.keyword.cloud_notm}}. Region 1 hosts the protected environment, and Region 2 hosts the recovery environment. Potentially, the recovery region can also host development and test workloads that can be “sacrificed” when a disaster recovery is started or tested.
    - Multiple ESXi bare metal servers form a cluster to host your virtual machines.
- **Zerto Virtual Manager (ZVM):**
    - Manages the replication between the protection and recovery sites, except for the actual replication of data.
    - Runs as a dedicated linux appliance and requires access to the Internet for licensing.
    - Interacts with the vCenter Server Appliance to get the inventory of VMs, disks, networks, and hosts.
    - Monitors changes in the hypervisor environment and responds.
    - Unless the sizing and performance considerations suggest otherwise, the application is installed with embedded SQL Server (localdb) as the database, providing a robust and efficient data storage solution.
- **Zerto Virtual Replication Appliance (VRA):**
    - Manages the replication of data from protected virtual machines to the recovery site.
    - Linux-based virtual machines that are installed on every hypervisor hosting protected VMs and every  hypervisor hosting replicated VM in the recovery site, ensuring VRAs hosted across both sites for VM replication.
    - Compresses the data that is passed across the network from the protected site to the recovery site. The VRA automatically adjusts the compression level according to CPU usage, including totally disabling it if needed.
    - Zerto recommends installing a VRA on every hypervisor host so that if protected virtual machines are moved from one host to another host in the cluster to ensure a VRA to protect the moved virtual machines.
- **Virtual Backup Appliance (VBA):**
    - Manages File Level Recovery operations within Zerto Virtual Replication.
    - Runs as a windows service on the ZVM.
    - Repositories can be local or on a shared network.
- **Data Streaming Service (DSS):**
    - Service running on the VRA, responsible for all the retention data path operations.
- **User interface:**
    - Recovery is managed in a browser or in the VMware vSphere web client or client console.

## Design Scope
{: #design-scope}

Consider the following considerations when you review the pattern:

- Network connectivity from on-premises to the {{site.data.keyword.cloud_notm}} environments is considered as out of scope for this pattern.
- Operation of your VMs is not impacted by Zerto. Zerto captures change data while it is still in the ESX host memory on its way to a data store (except during the initial sync or a delta sync)

For more information, see [Technical_Specifications](https://cloud.ibm.com/docs/vmwaresolutions?topic=vmwaresolutions-addingzertodr){: external} and [Zerto Scale and Benchmarking Guidelines](https://help.zerto.com/bundle/Scale.Bench.Guide.HTML/page/Zerto_Scale_and_Benchmarking_Guide_R.htm){: external}.

## IBM Architecture Framework
{: #reference-architecture-framework}

Following the IBM Architecture Framework, the Zerto deployment for disaster recovery for VMware Cloud Foundation (VCF) workloads pattern covers design and architecture decisions for the following aspects and domains:

- Compute: Virtual Servers
- Storage: Primary, Backup
- Networking: Public Connectivity, Segmentation, and Isolation, DNS
- Security: Data Security, Identity and Access Management
- Resiliency: High Availability, Disaster Recovery
- Service Management: Monitoring, Logging, Auditing, Alerting, Event Management

![img](image/heat-map-Zerto.svg){: caption="Figure 2. Architecture framework for Zerto deployment on VMware on {{site.data.keyword.cloud_notm}}" caption-side="bottom"}

The Architecture Framework offers a consistent approach to designing cloud solutions by addressing requirements across various technology-agnostic architectural areas, called "aspects" and "domains." For more information, see [Architecture framework](https://test.cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro)

### Requirements
{: #reference-architecture-requirements}

| **Aspect**                                                                                                                                                             | **Requirement**                                                                                                                                                                                               |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Compute                                                                                                                                                                      | CPU and RAM to support Zerto components                                                                                                                                                                             |
| Storage                                                                                                                                                                      | Storage to support Zerto components and to replicate the protected virtual machines                                                                                                                                 |
| Resiliency                                                                                                                                                                   | Replicate VMware workloads from a protected site to a recovery site in a different region for failover of workloads if a protected site failure occurs. Failover that meets the required RTO/RPO of the application |
| Service Management                                                                                                                                                           | Monitor the usage and performance of the Zerto components. Enable logging and alerting to DevOps tooling                                                                                                            |
{: caption="Table 1. Zerto Disaster Recovery solution requirements for VMware Workloads on {{site.data.keyword.cloud_notm}} VMware Foundation on VPC" caption-side="bottom"}

### Components
{: #comonents}

| **Aspect**                                                                                                                                                                  | **Component**                                 | **How the component is used**                                                                                                                                                                                                                                                          |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Data                                                                                                                                                                              | Embedded localdb installed with ZVM                 | Used as the Zerto Replication configuration database. Alternatively, an external SQL Server instance might be used. To use an externally managed database, select the Custom Installation option during the installation.                                                                    |
| Compute                                                                                                                                                                           | Virtual Machine                                     | Compute for ZVM and VRAs                                                                                                                                                                                                                                                                     |
| Storage                                                                                                                                                                           | vSAN                                                | Storage for ZVM and VRAs                                                                                                                                                                                                                                                                     |
| Networking                                                                                                                                                                        | Enterprise Connectivity                             | Connectivity to on-premises locations (*considered as out of scope for this pattern*)                                                                                                                                                                                                |
|                                                                                                                                                                                   | {{site.data.keyword.cloud_notm}} Backbone           | The {{site.data.keyword.cloud_notm}} private network is used to replicate traffic between the regions. Control traffic between the ZVM and the data-plane VRA components also traverses this network.                                                                                        |
|                                                                                                                                                                                   | Internet                                            | Internet access to connect to Zerto CallHome Server, Zerto Analytics and Zerto support.                                                                                                                                                                                                     |
| **Resiliency**                                                                                                                                                              | Zerto                                               | VMware virtual machines - Zerto provides the resiliency  Zerto data-plane components resiliency accomplished by deploying multiple VRAs. ZVM resiliency provided vSphere HA and database backups.   |
| **Service Management**                                                                                                                                                      | **Optional** - Zerto Analytics, Cloud Control | Zerto Analytics and Cloud Control provide visibility into Zerto-protected workloads and provide monitoring, reporting, alerting, diagnostics with automated resolutions and infrastructure utilization, and capacity planning.                                                                |
| {: caption="Table 2. Zerto Disaster Recovery solution components for VMware Workloads on {{site.data.keyword.cloud_notm}} VMware Cloud Foundations on VPC" caption-side="bottom"} |                                                     |                                                                                                                                                                                                                                                                                              |
