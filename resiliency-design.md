---
copyright:
  years: 2024
lastupdated: "2024-03-14"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Resiliency design
{: #resiliency-design}

## Requirements
{: #requirements}

The following are requirements for the resiliency aspect for the Zerto for disaster recovery for VMware workloads pattern:

- Replicate VMware workloads from a protected site to a recovery site in a different region to enable the failover of workloads in the event of failure in the protected site.
- Failover that meets the required RTO/RPO of the application.

![Zerto_solution_for_vSphere_architecture](image/Zerto-Architecture-High-Level.svg){: caption="Zerto_solution_for_vSphere_architecture" caption-side="bottom"}

Figure 1. Zerto solution for vSphere architecture

## Considerations
{: #considerations}

- Zerto replication is software-based and occurs in the hypervisor layer.
- The VRA continuously replicates data from VMs and VMDKs selected by the user, compressing it and sending it to the remote site over the {{site.data.keyword.cloud_notm}} backbone.
- Every write to a protected virtual machine is copied by Zerto Virtual Replication. The write is processed normally on the protected site and asynchronously replicated to the recovery site via a VRA journal. Each protected virtual machine has its own journal.
- Replication is near-synchronous.
- Zerto replicates all VMs belonging to an application (defined in Virtual Protection Groups, VPGs) at the same consistent checkpoint, regardless of the number of disks or VMs.

### Recovery Scenarios
{: #recovery scenarios}

* General guidance only, as each customer's workload and scenario are unique.

Scenario 1 : A limited number of VMs become unavailable/corrupted in the production site

- By using Zerto's re-IP features, recovery of individual applications or VMs is possible when used with DNS updates. See [How the Re-IP works in Zerto](https://help.zerto.com/kb/000002926).
- Depending on your network routing and subnet allocations, failover without re-IP is also possible.

Scenario 2 : The VMware environment in the protected site becomes unavailable:

- Enable your routing so that NSX overlay IP address ranges at the protected site are routed to the recovery site.
- Perform Zerto failover of protected VMs/applications at a specified checkpoint using the recovery site ZVM (for more information on this topic: [Failover_Operation](https://help.zerto.com/bundle/Admin.VC.HTML.10.0_U3/page/The_Failover_Operation.htm)).
- When the protected site becomes available, enable reverse protection and perform a Zerto move operation from the recovery site to the protected site (for more information on this topic: [Move_Operation](https://help.zerto.com/bundle/Admin.ZSSP.HTML.10.0_U3/page/The_Move_Operation.htm))

Scenario 3 : Recovery of specific files or folders in a protected VM.

- You can recover specific files and folders on Windows or Linux virtual machines that are being protected by Zerto. Recover files/folders from short-term restore point or Extended Journal Copy Repository (recovery site only). See [Restoring Files and Folders](https://help.zerto.com/bundle/Admin.VC.HTML.10.0_U3/page/restore.htm)
