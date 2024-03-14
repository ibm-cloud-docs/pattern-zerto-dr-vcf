---
copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: <pattern-zerto-dr-vcf>

keywords:
---

{{site.data.keyword.attribute-definition-list}}

# Resiliency design

{: \#resiliency-design}

This section expands on the resilency aspect of the IBM Architecture framework in respect of the Zerto disaster recovery for VMware Cloud Foundation in IBM Cloud VPC pattern.

## Requirements

The requirements for the resiliency aspect for the Zerto for disaster recovery for VMware workloads pattern focus on the following:

-   Replicate VMware workloads from a protected site to a recovery site in a different region to enable the failover of workloads in the event of failure in the protected site.
-   Failover that meets the required RTO/RPO of the application.

![Zerto_solution_for_vSphere_architecture](image/Zerto-Architecture-High-Level.svg)

Figure 1. Zerto solution for vSphere architecture

## Considerations

-   Zerto replication is software-based and occurs in the hypervisor layer.
-   The VRA continuously replicates data from VMs and VMDKs selected by the user, compressing it and sending it to the remote site over the IBM Cloud backbone.
-   Every write to a protected virtual machine is copied by Zerto Virtual Replication. The write continues to be processed normally on the protected site and the copy is sent asynchronously to the recovery site and written to a journal managed by a Virtual Replication Appliance (VRA). Each protected virtual machine has its own journal.
-   Replication is near-synchronous.
-   Zerto replicates all VMs belonging to an application (defined in Virtual Protection Groups, VPGs) at the same consistent checkpoint, regardless of the number of disks or VMs.

### Recovery Scenarios

Listed below are some high-level steps typically needed to recover the protected VMware workloads from a disaster, these steps are only provided for general guidance as each customer’s workload and scenarios are unique.

Scenario 1 : A limited number of VMs become unavailable/corrupted in the production site

-   By using Zertos re-IP features, recovery of individual applications or VMs is possible when used with DNS updates. See [How the Re-IP works in Zerto](https://help.zerto.com/kb/000002926).
-   Depending on your network routing and subnet allocations, failover without re-IP is also possible.

Scenario 2 : The VMware environment in the protected site becomes unavailable:

-   Enable your routing so that NSX overlay IP address ranges at the protected site are routed to the recovery site.
-   Using the recovery site ZVM, perform a zerto failover operation of the protected VMs or application to be recovered, specifying the checkpoint to which the VMs are to be recovered (for more information see: [Failover_Operation](https://help.zerto.com/bundle/Admin.VC.HTML.10.0_U3/page/The_Failover_Operation.htm)).
-   When the protected site becomes avaible, enable reverse protection and perform a zerto move operation from the recovery site to the protected site (for more information see: [Move_Operation](https://help.zerto.com/bundle/Admin.ZSSP.HTML.10.0_U3/page/The_Move_Operation.htm))

Scenario 3 : Recovery of specific files or folders in a protected VM.

-   You can recover specific files and folders on Windows or Linux virtual machines that are being protected by Zerto. You can recover the files and folders from a short-term restore point of time specified in the Journal, or from a Extended Journal Copy Repository (the latter will be performed from the Recovery site only). See [Restoring Files and Folders](https://help.zerto.com/bundle/Admin.VC.HTML.10.0_U3/page/restore.htm)
