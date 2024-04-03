---
copyright:
  years: 2024
lastupdated: "2024-03-29"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Storage design
{: #storage-design}

## Requirements
{: #storage-requirements}

The following requirements are for the storage aspect for the Zerto for disaster recovery for VMware workloads pattern:

- Storage to support the Zerto components ZVM and VRA
- Storage to support VM replicas

## Journal
{: #journal}

Zerto uses a journal to track the changes that are occurring on the protected VMs. The Zerto journal is where replication data is stored during ongoing replication between sites. Journaling enables recovery points if there is a failure or rollback to previous points in time. Zerto recommends:

- Journal storage should be accessible by all the recovery hosts, not just by one of the hosts.
- The sizing of the journal depends on the amount of data that is replicated, the replication update frequency, and retention period.
- Zerto recommends setting aside 10-20% of the total data replication size for journal storage. For example, if replicating 1 TB of VM data, provision 100-200 GB for the journal.
- The journal storage needs fast input/output operations per second (IOPS) performance since it is actively written to during replication. Solid-state drives (SSDs) are recommended for best performance.
- Each protected virtual machine has its own dedicated journal. The journal associated to a protected VM is stored by default on the storage that is used for the recovery VM on the recovery site.
- For retention, the journal grows over time as changes accumulate. Zerto has compression and pruning techniques to manage size over time based on retention policies.
- Regularly testing recovery is recommended to validate that sufficient journal storage is provisioned based on actual workload requirements.

## Sizing-Journal
{: #journal-sizing}

To calculate the journal size, you can use the following formula:

Journal size = (data change rate per hour x retention policy in hours) + 10%

For example, if you are replicating 100 GB of data that changes at a rate of 10 GB per hour, and you want to retain a 24-hour history, the journal size would be:

(10 GB/hour x 24 hours) + 10% = 264 GB

So, you would need at least 264 GB of free storage space on the target site to accommodate the Zerto journal for this replication scenario.

For more information, see [Journal Overview_ Sizing and Best Practice](https://help.zerto.com/bundle/BP.Journal.Sizing.HTML/page/Journal_Overview__Sizing_and_Best_Practice.htm){: external}.

## Considerations
{: #storage-considerations}

- The ZVMs and VRAs are deployed in the VCF environment, they use storage from the provisioned data store, which in the case of VCF is vSAN based.
- Properly size the recovery site VMware deployment data stores to accommodate the replicas. Scaling a vSAN cluster involves adding more bare metal ESXis to the cluster.
