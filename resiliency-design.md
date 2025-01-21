---
copyright:
  years: 2024
lastupdated: "2024-03-29"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Resiliency design
{: #resiliency-design-id}

## Requirements
{: #resiliency-requirements}

The following are requirements for the resiliency aspect of the Zerto for disaster recovery for VMware workloads pattern:

- Replicate VMware workloads from a protected site to a recovery site in a different region to enable the failover of workloads if there is a failure in the protected site.
- Failover that meets the required Recovery Time Objectives (RTO) and Recovery Point Objectives (RPO) of the application.

![Zerto_solution_for_vSphere_architecture](image/Zerto-Architecture-High-Level.svg){: caption="Zerto solution for vSphere architecture" caption-side="bottom"}

## Considerations
{: #resiliency-considerations}

- Zerto replication is software-based and occurs in the hypervisor layer.
- The VRA continuously replicates data from VMs and VMDKs selected by the user, compressing it and sending it to the remote site over the {{site.data.keyword.cloud_notm}} backbone.
- Every disk write operation to a protected virtual machine is copied by using Zerto Virtual Replication. The write is processed normally on the protected site and asynchronously replicated to the recovery site by a VRA journal. Each protected virtual machine has its own journal.
- Replication is near-synchronous.
- Zerto replicates all VMs belonging to an application, which is defined in Virtual Protection Groups (VPGs), at the same consistent checkpoint, regardless of the number of disks or VMs.

### Recovery Scenarios
{: #recovery-scenarios}

The following scenarios are general guidance only. Customer workloads and scenarios are unique.

**Scenario 1** : A limited number of VMs become unavailable/corrupted in the production site

- By using Zerto's re-IP features, recovery of individual applications or VMs is possible when used with DNS updates. For more information, see [How the Re-IP works in Zerto](https://help.zerto.com/kb/000002926){: external}.
- Depending on your network routing and subnet allocations, failover without re-IP is also possible.

**Scenario 2** : The VMware environment in the protected site becomes unavailable

- Enable your routing so that NSX overlay IP address ranges at the protected site are routed to the recovery site.
- Perform Zerto failover of protected VMs and applications at a specified checkpoint by using the recovery site ZVM. For more information, see [Failover_Operation](https://help.zerto.com/bundle/Admin.VC.HTML.10.0_U3/page/The_Failover_Operation.htm){: external}.
- When the protected site becomes available, enable reverse protection and perform a Zerto move operation from the recovery site to the protected site. For more information, see [Move_Operation](https://help.zerto.com/bundle/Admin.ZSSP.HTML.10.0_U3/page/The_Move_Operation.htm){: external}.

**Scenario 3** : Recovery of specific files or folders in a protected VM

- You can recover specific files and folders on Windows or Linux virtual machines that are being protected by Zerto. Recover files and folders from short-term restore point or Extended Journal Copy Repository (recovery site only). See [Restoring Files and Folders](https://help.zerto.com/bundle/Admin.VC.HTML.10.0_U3/page/restore.htm){: external}.
