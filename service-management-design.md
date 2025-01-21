---
copyright:
  years: 2024
lastupdated: "2024-03-29"

subcollection: pattern-zerto-dr-vcf

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Service management design considerations
{: #service}

## Requirements
{: #service-management-requirements}

The following are requirements for the compute aspect for the Zerto for disaster recovery for VMware workloads pattern:

- Monitor the usage and performance of the Zerto components.
- Enable logging and alerting to DevOps tools.

Monitoring a Zerto replication environment is crucial to ensure the health, performance, and reliability of the replication processes. Ensure effective monitoring of your Zerto replication environment by following best practices.

## Considerations
{: #monitoring-considerations}

Review the key considerations for service management.

### Infrastructure monitoring
{: #infrastructure-monitoring}

- Monitor the underlying infrastructure components, including virtualization hosts, storage, and network devices, to identify potential bottlenecks. Consider the use of the {{site.data.keyword.cloud_notm}} console or [{{site.data.keyword.monitoringlong_notm}}. For more information see [{{site.data.keyword.monitoringlong_notm}}](/docs/monitoring?topic=monitoring-getting-started) or  [Monitoring for VMware vCenter Server deployments](/docs/monitoring?topic=monitoring-vmware-vcenter).
- Use performance monitoring tools specific to your virtualization platform to track resource utilization. Review Zerto analytics for performance monitoring, or consider VMware Aria operations and logs on {{site.data.keyword.IBM_notm}}. For more information, see  [VMware Aria Operations and VMware Aria Operations for Logs on {{site.data.keyword.cloud_notm}} overview](/docs/vmwaresolutions?topic=vmwaresolutions-vrops_overview).

### IBM Cloud Logs:
{: #cloud-log}

- Regularly review Zerto log files for any warning or error messages. Logs can provide detailed information about the health and performance of various components. For more information, see [Collecting Zerto logs](https://help.zerto.com/bundle/Admin.VC.HTML.95/page/Collecting_Zerto_Logs.htm){: external}.
- Configure log retention and archiving to ensure that historical logs are available for troubleshooting and analysis.
- Consider the use of {{site.dat.keyword.logs_full_notm}}. For more information, see [Getting started with IBM Cloud Logs](/docs/cloud-logs).
- Consider the optional add-on service [VMware Aria Operations and VMware Aria Operations for Logs on {{site.data.keyword.cloud_notm}} overview](/docs/vmwaresolutions?topic=vmwaresolutions-vrops_overview).

### Event Logs and Syslog Integration:
{: #logintegration}

- Integrate Zerto with centralized logging systems or syslog servers to consolidate and analyze logs across the environment. For more information, see [syslog](https://help.zerto.com/kb/000003918){: external}.
- Monitor Windows event logs for events related to Zerto and address any issues promptly. This is performed by using {{site.data.keyword.logs_full_notm}}.
- All alerts that are issued by Zerto are displayed in the Zerto User Interface. For more information, see [Zerto alerts](https://help.zerto.com/bundle/Alarms.Alerts.HTML/page/Zerto_Alerts.htm){: external}.
- Zerto Analytics provides comprehensive monitoring and health tracking for your protected environment. Using Zerto analytics, you can see aggregated information from the Zerto Virtual Managers and view the status of your environment. All your alerts, tasks, events, and information on Virtual Protection Groups (VPGs) can be viewed together. Zerto analytics is developed with an API first approach. Everything that is presented in the GUI is also available with APIs. The APIs are delivered with Swagger open source that help you develop, test REST integration that uses standardized examples, and easily populate custom portals with Zerto analytics content. For more information, see [Zerto Analytics](https://help.zerto.com/bundle/Zerto.Analytics.HTML/page/Zerto_Analytics_-_Overview_and_Use.htm){: external}.
- Zerto can issue alerts through the vCenter's alarms feature. The actions that are triggered by these alarms can be customized by the administrator. For more information about how to configure these actions, which include sending an email, a notification trap, or running a command from the vCenter server, see [VMware alerts](https://help.zerto.com/bundle/Alarms.Alerts.HTML/page/Zerto_Alarms_In_VMware_vSphere.htm){: external}.
- By default, Zerto stores its logs locally on the host where it is installed. For more information, see [Understanding Logs](https://help.zerto.com/bundle/Admin.VC.HTML.97/page/Understanding_the_Logs.htm){: external}.
- Email notifications or SNMP traps can also be used for alerting or job monitoring. For more information, see [Zerto email alerts](https://help.zerto.com/kb/000003529){: external}.

### Network Monitoring
{: #networkmonitoring}

- Monitor network traffic between the primary and secondary sites to ensure that replication traffic is not impeded. For more information, see [Monitoring Peer Sites](https://help.zerto.com/bundle/Admin.VC.HTML.90/page/Monitoring_Peer_Sites_%E2%80%93_The_SITES_Tab.htm){: external}.
- Set up alerts for unusual network behavior or performance issues.

### Regular Audits and Reviews
{: #audit-reviews}

- Conduct regular audits of your Zerto configuration and performance to identify areas for optimization and enhancement.
- Review reports and alerts to ensure that any issues are addressed in a timely manner.

### User Activity Monitoring
{: #user-monitoring}

- Monitor user activities within the Zerto environment, such as changes to policies, to ensure security and compliance. For more information, see [Monitoring Tasks](https://help.zerto.com/bundle/Admin.Azure.HTML.90/page/Monitoring_Tasks.htm){: external}.

### Automation
{: #automation-considerations}

- The automated deployment of VMware Cloud Foundation on {{site.data.keyword.vpc_short}} does not include any Zerto components. You need to manually install and configure the required Zerto components.

- To obtain an image to install the required software in this architecture pattern, open an {{site.data.keyword.cloud_notm}} support case to {{site.data.keyword.IBM_notm}} for VMware Solutions.
{: important}

- Manage the protection and replication of virtual machines between the protected and recovery sites, by using the Zerto User Interface.
- Zerto also provides a set of RESTful APIs and PowerShell cmdlets ([Zerto powershell command module](https://help.zerto.com/bundle/Powershell.CMDlets.HTML.90/page/Introduction.htm){: external}) to enable incorporating some of the disaster recovery functions within scripts or programs. For more information, see [Zerto Cloud Manager Restful APIs](https://help.zerto.com/bundle/API.ZCM.HTML.10.0_U3/page/Introduction_to_the_ZCM_RESTful_APIs.htm){: external}.
