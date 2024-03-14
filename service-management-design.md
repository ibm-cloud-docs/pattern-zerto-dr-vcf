---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-zerto-dr-vcf

keywords:
---
# Service management design consideration

{: #service}

This section expands on the service management aspect of the IBM Architecture framework in respect of the Zerto disaster recovery for VMware Cloud Foundation in {{site.data.keyword.vpc_short}} pattern.

## Requirements

{: #requirements}

The requirements for the compute aspect for the Zerto for disaster recovery for VMware workloads pattern focus on the following:

- Monitor the usage and performance of the Zerto components.
- Enable logging and alerting to DevOps tooling.

Monitoring a Zerto Replication environment is crucial to ensure the health, performance, and reliability of the replication processes. Here are some guidance and best practices for monitoring a Zerto replication environment.

## Considerations

{: #considerations}

Some key considerations for service management are listed below.

### **Infrastructure monitoring:**

- Monitor the underlying infrastructure components, including virtualization hosts, storage, and network devices, to identify potential bottlenecks. Consider the use of the {{site.data.keyword.cloud_notm}}console see [{{site.data.keyword.monitoringlong_notm}}](https://test.cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-monitoring-iaas) or {{site.data.keyword.cloud_notm}}services such as {{site.data.keyword.monitoringlong_notm}}, see [Monitoring for VMware vCenter Server deployments](https://test.cloud.ibm.com/docs/monitoring?topic=monitoring-vmware-vcenter).
- Use performance monitoring tools specific to your virtualization platform to track resource utilization. Review the use of Zerto Analytics as your performance monitoring tool, see [VMware vSphere Monitoring](https://helpcenter.veeam.com/docs/one/monitor/vsphere_monitoring.html?ver=120) or consider the optional add-on service [VMware Aria Operations and VMware Aria Operations for Logs on {{site.data.keyword.cloud_notm}}overview](https://test.cloud.ibm.com/docs/vmwaresolutions?topic=vmwaresolutions-vrops_overview)

### **Log Analysis:**

- Regularly review Zerto log files for any warning or error messages. Logs can provide detailed information about the health and performance of various components. See [Collecting Zerto logs](https://help.zerto.com/bundle/Admin.VC.HTML.95/page/Collecting_Zerto_Logs.htm).
- Configure log retention and archiving to ensure that historical logs are available for troubleshooting and analysis.
- Consider the use of {{site.data.keyword.loganalysislong_notm}}. See [Getting started with {{site.data.keyword.loganalysislong_notm}}](https://test.cloud.ibm.com/docs/log-analysis?topic=log-analysis-getting-started). See [Logging for VMware vCenter Server deployments](https://test.cloud.ibm.com/docs/log-analysis?topic=log-analysis-vmware-vcenter).
- Consider the optional add-on service [VMware Aria Operations and VMware Aria Operations for Logs on {{site.data.keyword.cloud_notm}}overview](https://test.cloud.ibm.com/docs/vmwaresolutions?topic=vmwaresolutions-vrops_overview).

### **Event Logs and Syslog Integration:**

- Integrate Zerto with centralized logging systems or syslog servers to consolidate and analyze logs across the environment. See [syslog](https://help.zerto.com/kb/000003918).
- Monitor Windows Event Logs for events related to Zerto and address any issues promptly.
- This is performed using {{site.data.keyword.loganalysislong_notm}}.
- All alerts issued by Zerto are displayed in the Zerto User Interface. See [Zerto alerts](https://help.zerto.com/bundle/Alarms.Alerts.HTML/page/Zerto_Alerts.htm).
- Zerto Analytics allows you to track and monitor the health of your entire protected environment. Using Zerto Analytics, you can see aggregated information from the Zerto Virtual Managers, and view the status of your environment. All your alerts, tasks, events and information on Virtual Protection Groups (VPGs) can be viewed together. Zerto Analytics is developed with an API first approach, therefore, everything that is presented in the GUI, is also available with APIs. The APIs are delivered with Swagger open source that help you develop and test REST integration using standardized examples. This allows you to easily populate custom portals with Zerto Analytic content. See [Zerto Analytics](https://help.zerto.com/bundle/Zerto.Analytics.HTML/page/Zerto_Analytics_-_Overview_and_Use.htm).
- Zerto could issue alerts through the vCenter's Alarms feature. The actions triggered by these alarms can be customized by the administrator. This article will detail how to configure these actions, which include sending an email, a notification trap, or running a command from the vCenter server. See [VMware alerts](https://help.zerto.com/bundle/Alarms.Alerts.HTML/page/Zerto_Alarms_In_VMware_vSphere.htm).
- By default, Zerto store its logs locally on the host where it is installed (see [Understanding Logs](https://help.zerto.com/bundle/Admin.VC.HTML.97/page/Understanding_the_Logs.htm)).
- Email notifications or SNMP traps can also be used for alerting or job monitoring. See [Zerto email alerts](https://help.zerto.com/kb/000003529).

### **Network Monitoring:**

- Monitor network traffic between the primary and secondary sites to ensure that replication traffic is not impeded. See [Monitoring Peer Sites](https://help.zerto.com/bundle/Admin.VC.HTML.90/page/Monitoring_Peer_Sites_%E2%80%93_The_SITES_Tab.htm).
- Set up alerts for unusual network behavior or performance issues.

**Regular Audits and Reviews:**

- Conduct regular audits of your Zerto configuration and performance to identify areas for optimization and enhancement.
- Review reports and alerts to ensure that any issues are addressed in a timely manner.

**User Activity Monitoring:**

- Monitor user activities within the Zerto environment, such as changes to policies, to ensure security and compliance. See [Monitoring Tasks](https://help.zerto.com/bundle/Admin.Azure.HTML.90/page/Monitoring_Tasks.htm).

**Automation**:

- The automated deployment of VMware Cloud Foundation on {{site.data.keyword.vpc_short}} does not include any Zerto components. Therefore, you need to manually install and configure the required Zerto components. **To obtain an image to install the required software in this architecture pattern, raise an {{site.data.keyword.cloud_notm}}Support ticket to IBM Cloud® for VMware Solutions.**
- Manage the protection and replication of virtual machines between the protected and recovery sites, using the Zerto User Interface.
- Zerto also provides a set of RESTful APIs and PowerShell cmdlets to enable incorporating some of the disaster recovery functionality within scripts or programs. See [Zerto Cloud Manager Restful APIs](https://help.zerto.com/bundle/API.ZCM.HTML.10.0_U3/page/Introduction_to_the_ZCM_RESTful_APIs.htm).
