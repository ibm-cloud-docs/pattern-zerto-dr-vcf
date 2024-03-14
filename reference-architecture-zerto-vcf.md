---
copyright:
  years: 2023
lastupdated: "2024-02-24"

subcollection: pattern-zerto-dr-vcf
keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Deploy {{site.data.keyword.cloud_notm}} resiliency design with Zerto on {{site.data.keyword.cloud_notm}} VMware
{: #resiliency-zerto}

This pattern describes Zerto for disaster recovery for VMware Cloud Foundations (VCF) workloads where both the protected and recovery sites are in {{site.data.keyword.vpc_short}}. The deployment of the Zerto solution will need to be done manually, however, Zerto licenses can be ordered through VMware Solutions portal.

- Disaster recovery is a shared responsibility of the Client and supported by IBM/Partner for building and supported day 2 services. If you do not use IBM/Partner for building and supported day 2 services, then you are totaly responsible for disaster recovery.
- If the workload to be protected must support data encryption, GPDR and other regulated compliance please refer to [Deploy {{site.data.keyword.cloud_notm}} Resiliency Design with Veeam on VMware](https://cloud.ibm.com/docs/vmware-cross-region-dr?topic=vmware-cross-region-dr-overview).
- This pattern is cross-region, meaning that the recovery site is in a different {{site.data.keyword.cloud_notm}} region than the protected location e.g. protected site is Frankfurt and the recovery location is Madrid. However, if required the pattern can use a recovery site in the same geographic region, but a different Availability Zone if required e.g. Frankfurt AZ1 and Frankfurt AZ3.

Check to ensure that the minimum distance between the protected and recovery sites meets your requirement. {: important}

This pattern builds on Zerto best-practice guidance, see [Zerto KB](https://help.zerto.com/category/Best_Practices).

VCF in {{site.data.keyword.vpc_short}} can be deployed as one of two architectures; standard and consolidated. This pattern is based on the consolidated architecture but includes additional considerations for the standard architecture.{: note}

## Zerto Architecture diagram
{: #resiliency-architecture}

The following diagram describes the high-level steps to deploy Zerto on a VMware Cloud Foundation deployed with the consolidated architecture. In this pattern, Zerto appliances are deployed into the management domain as virtual machines (VMs). This architecture pattern deployment is summarized as follows:

1. Create a bare metal server VLAN interface into management subnet for Zerto ZVM. This step provides an IP address from the management subnet for use for the Zerto ZVM. Attach to equivalent management security groups. Add required DNS A and PTR records to the DNS service according to the Zerto documentation and your solution requirements.
2. Deploy Zerto ZVM appliance and attach it to the management DPG by using the provisioned IP address. Plan and size your deployment. Obtain a license through the VMware Solutions console.
3. Create the required number of bare metal server VLAN interfaces with reserved IP addresses by using consecutive IP range into management subnet for Zerto VRAs. Attach to equivalent management security groups in Virtual Private Cloud (VPC).
4. Configure Zerto ZVM with the IP address range and using the workflow in ZVM, deploy the VRAs.

![img](image/Zerto-Architecture-Disaster-recovery.svg){: caption="Zerto Architecture Disaster Recovery" caption-side="bottom"}

Figure 1. Zerto Disaster Recovery for VMWare Solution Components

For the reference architecture described below we assume you have already deployed a consolidated VCF deployment in two Regions.

The key features of this pattern are as follows:

- **{{site.data.keyword.cloud_notm}} Infrastructure:**
   - Two VMware Cloud Foundation environments hosted on {{site.data.keyword.cloud_notm}}. Region 1 hosts the protected environment, and Region 2 hosts the recovery environment. Potentially the recovery region can also host development and test workloads that can be “sacrificed” when a disaster recovery is invoked or tested.
   - Multiple ESXi bare metal servers forming a cluster hosting your virtual machines.
- **Zerto Virtual Manager (ZVM):**
   - Manages everything required for the replication between the protection and recovery sites, except for  the actual replication of data.
   - Running as a dedicated linux appliance and requires access to the Internet for licenceing.
   - Interacts with the vCenter Server Appliance to get the inventory of VMs, disks, networks, hosts, etc.
   - Monitors changes in the hypervisor environment and responds accordingly.
   - Installed with embedded SQL Server (localdb) as the database unless sizing and performance indicate otherwise.
- **Zerto Virtual Replication Appliance (VRA):**
   - Manages the replication of data from protected virtual machines to the recovery site.
   - Linux-based virtual machine installed on every hypervisor that hosts virtual machines that require protecting in the protected site and on every hypervisor that will host the replicated virtual machines in the recovery site, this is essential that VRA are hosted across **protected and recovery site for VM replication.**
   - Compresses the data that is passed across the network from the protected site to the recovery site. The VRA automatically adjusts the compression level according to CPU usage, including totally disabling it if needed.
   - Zerto recommends installing a VRA on every host so that if protected virtual machines are moved from one host in the cluster to another host in the cluster there is always a VRA to protect the moved virtual machines.
- **Virtual Backup Appliance (VBA):**
   - Manages File Level Recovery operations within Zerto Virtual Replication.
   - Windows service running on the ZVM.
   - Repositories can be local or on a shared network.
- **Data Streaming Service (DSS):**
   - Service running on the VRA, responsible for all the retention data path operations.
- **User interface:**
   - Recovery using the Zerto solution is managed in a browser or, in VMware vSphere Web Client or Client console.

## Design Scope
{: #designscope}

Consider the following when reviewing the pattern:

- Network connectivity from on-premises to the {{site.data.keyword.cloud_notm}} environments is considered as out of scope for this pattern.
- Zerto will not impact the operation of your VMs. This is because Zerto captures change data while it is still in the memory of the ESX host on its way to a datastore. The only time this is not the case is during the initial sync or a delta sync.
- For more information on the Zerto components see the following:
   - [Technical_Specifications](https://cloud.ibm.com/docs/vmwaresolutions?topic=vmwaresolutions-addingzertodr).
   - [Zerto_Scale_and_Benchmarking_Guidelines](https://help.zerto.com/bundle/Scale.Bench.Guide.HTML/page/Zerto_Scale_and_Benchmarking_Guide_R.htm).

## IBM Architecture Framework
{: #architecture}

Following the IBM Architecture Framework, the VMware VCF in {{site.data.keyword.vpc_short}} disaster recovery solution using Zerto covers design considerations and architecture decisions for the following aspects and domains:

- Compute: Virtual Servers.
- Storage: Primary Storage, Backup.
- Networking: Public Connectivity, Segmentation and Isolation, DNS.
- Security: Data Security, Identity and Access Management.
- Resiliency: High Availability, Disaster Recovery.
- Service Management: Monitoring, Logging, Auditing, Alerting.

![img](image/heat-map-Zerto.svg) Figure 2 Architecture framework for Zerto deployment on VMware on {{site.data.keyword.cloud_notm}}

The IBM Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. For more details, see [Architecture framework](https://test.cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro)

### Requirements
{: #requirements}

| **Aspect**   | **Requirement**                                                                                                                                                                                                          |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Compute            | CPU and RAM to support Zerto components                                                                                                                                                                                        |
| Storage            | Storage to support Zerto components and to replicate the protected virtual machines                                                                                                                                            |
| Resiliency         | Replicate VMware workloads from a protected site to a recovery site in a different region for failover of workloads in the event of failure in the protected site. Failover that meets the required RTO/RPO of the application |
| Service Management | Monitor the usage and performance of the Zerto components. Enable logging and alerting to DevOps tooling                                                                                                                       |
{: caption="Table 1. Zerto Disaster Recovery solution requirements for VMware Workloads on {{site.data.keyword.cloud_notm}} VMware Foundation on VPC" caption-side="bottom"}


### Components
{: #components}

| **Aspect**             | **Component**                                 | **How the component is used**                                                                                                                                                                                                                                                          |
| ---------------------------- | --------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Data                         | Embeded localdb installed with ZVM                  | Used as the Zerto Replication configuration database. Alternatively, an external SQL Server instance could be used. To use an externally managed database, select the Custom Installation option during the installation.                                                                    |
| Compute                      | Virtual Machine                                     | Compute for ZVM and VRAs                                                                                                                                                                                                                                                                     |
| Storage                      | vSAN                                                | Storage for ZVM and VRAs                                                                                                                                                                                                                                                                     |
| Networking                   | Enterprise Connectivity                             | Connectivity to on-premises locations (**considered as out of scope for this pattern**)                                                                                                                                                                                                |
|                              | {{site.data.keyword.cloud_notm}} Backbone                                  | The {{site.data.keyword.cloud_notm}} private network is used to replicate traffic between the regions. Control traffic between the ZVM and the data-plane VRA components also traverses this network.                                                                                                               |
|                              | Internet                                            | Internet access to connect to Zerto CallHome Server, Zerto Analytics. and Zerto support.                                                                                                                                                                                                     |
| **Resiliency**         | Zerto                                               | Zerto provides the resiliency of the VMware virtual machines to be protected and recovered. The resiliency of the Zerto data-plane components is accomplished by deploying multiple VRAs. For the ZVM, vSphere HA and backups of the database enables resiliency of the Zerto control-plane. |
| **Service Management** | **Optional** - Zerto Analytics, Cloud Control | Zerto Analytics and Cloud Control provide visibility into Zerto-protected workloads and provide monitoring, reporting, alerting, diagnostics with automated resolutions and infrastructure utilization and capacity planning.                                                                |
{: caption="Table 2. Zerto Disaster Recovery solution components for VMware Workloads on {{site.data.keyword.cloud_notm}} VMware Cloud Foundations on VPC" caption-side="bottom"}
