---
copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Storage design

{: \#storage-design}

This section expands on the storage aspect of the IBM Architecture framework in respect of the Zerto disaster recovery for VMware Cloud Foundation in IBM Cloud VPC pattern.

## Requirements

The requirements for the storage aspect for the Zerto for disaster recovery for VMware workloads pattern focus on the following:

- Storage to support Zerto components (ZVM and VRA)
- Storage to support replicas (VM replicas)

## Journal

Zerto uses a journal to track the changes occuring on the protected VMs. The Zerto Journal is where replication data is stored during ongoing replication between sites. This allows for recovery points in case of failure or rollback to previous points in time. Zerto recommends:

- Journal storage should be accessible by all the recovery hosts and not just by one of the hosts.
- The journal size depends on the amount of data being replicated, the replication update frequency, and retention period.
- As a general guideline, Zerto recommends provisioning at least 10-20% of the total replicated data size for journal storage. For example, if replicating 1TB of VM data, provision 100-200GB for the journal.
- The journal storage needs fast IOPS performance since it is actively written to during replication. SSDs are recommended for best performance.
- Each protected virtual machine has its own dedicated journal. The journal associated to a protected VM is stored by default on the storage used for the recovery this VM (on the recovery site).
- For retention, the journal grows over time as changes accumulate. Zerto has compression and pruning techniques to manage size over time based on retention policies.
- Regularly testing recovery is recommended to validate sufficient journal storage is provisioned based on actual workload requirements.

## Journal Sizing

To calculate the journal size, you can use the following formula:

Journal size = (data change rate per hour x retention policy in hours) + 10%

For example, if you are replicating 100GB of data that changes at a rate of 10GB per hour, and you want to retain a 24-hour history, the journal size would be:

(10GB/hour x 24 hours) + 10% = 264GB

So you would need at least 264GB of free storage space on the target site to accommodate the Zerto Journal for this replication scenario.

See [Journal_Overview__Sizing_and_Best_Practice](https://help.zerto.com/bundle/BP.Journal.Sizing.HTML/page/Journal_Overview__Sizing_and_Best_Practice.htm)

## Considerations

- The ZVMs and VRAs are deployed in the VCF environment, so they will use storage from the provisioned datastore, which in the case of VCF is vSAN based.
- For replication, an important consideration, especially given that vSAN is being used for the VMware VCF on IBM Cloud datastores, is to properly size the recovery site VMware deployment datastores so that they can accommodate the replicas (as scaling a vSAN cluster involves adding additional bare metal ESXis to the cluster).
