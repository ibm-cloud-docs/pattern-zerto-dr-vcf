---
copyright:
  years: 2024
lastupdated: "2024-03-29"

subcollection: pattern-zerto-dr-vcf

keywords:
---
# Architecture decisions for service management
{: #ad-service-management}

The following sections summarize the architecture decisions for service management for the Zerto for VCF resiliency pattern.

## Monitoring Architecture Decisions
{: #architecture decisions for monitoring}

| Architecture decision                                                              | Requirement                                                                                              | Options                                                                                             | Decision                                                     | Rationale                                                                                                                                                                                                                                                   |
| ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Operational monitoring of cloud infrastructure and services                        | Monitor system health to detect issues that might impact the availability of the system and application. | - Zerto analytics  \n -  {{site.data.keyword.monitoringlong_notm}}  \n -  3rd party monitoring tool | Zerto analytics                                              | Zerto analytics is included in the service, which requires the creation of a Zerto account.                                                                                                                                                                 |
| Operational monitoring of applications                                             | Monitor app health to detect issues that might impact the availability of the app.                       | - {{site.data.keyword.monitoringlong_notm}}  \n -  Instana (SaaS)  \n -  3rd party monitoring tool  | {{site.data.keyword.monitoringlong_notm}} and Instana (SaaS) | Instana is used along with {{site.data.keyword.monitoringlong_notm}} to provide more application performance metrics and management automation. Instana provides data and actionable insights to monitor the applications and automate root-cause analysis. |
| {: caption="Table 1. Architecture decisions for monitoring" caption-side="bottom"}


## Logging Architecture decision
{: #ad-logging}

| Architecture decision                                                        | Requirement                                                                                                   | Options                                                                        | Decision                                     | Rationale                                                                                                                                                 |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Log monitoring of cloud infrastructure and services                          | Monitor operational logs to detect issues that might impact the availability of the system and application.   | - IBM Cloud Logs \n -  VMware Aria  \n -  3rd party logging tool              | IBM Cloud Logs                               | IBM Cloud Logs collects operational logs from applications, platform resources, and infrastructure and provides interfaces to view and analyze all logs. |
| Log monitoring of applications                                               | Monitor application operational logs to detect issues that might impact the availability of the applications. | - IBM Cloud Logs \n -  application logging tool  \n -  3rd party logging tool | IBM Cloud Logs and application logging tool | Use the application logging tool to send application logs to IBM Cloud Logs and aggregate application-specific log details.                              |
| Log monitoring of databases                                                  | Monitor database logs to detect issues that might impact the availability of the database.                    | - IBM Cloud Logs \n -  database logging tools  \n -  3rd party logging tool  | IBM Cloud Logs and database logging tool    | Use the database logging tools along with IBM Cloud Logs to get more database-specific log information.                                                  |
| {: caption="Table 2. Architecture decisions for logs" caption-side="bottom"}


## Alerting Architecture decision
{: #ad-alerting}

| Architecture decision                                                          | Requirement                                                                                                                           | Options                                                                                                                 | Decision                                                                                                                | Rationale                                                                                                                                                                                                                                                                               |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Operational alerts                                                             | Provide a mechanism to identify and send notifications about operational issues that are found across application and infrastructure. | - {{site.data.keyword.monitoringlong_notm}} \n * IBM Cloud Logs \n * Event Notifications                               | {{site.data.keyword.monitoringlong_notm}} \n * IBM Cloud Logs \n * Event Notifications                                 | {{site.data.keyword.monitoringlong_notm}} and IBM Cloud Logs support the configuration of alerts to detect operational issues and send notifications to targeted channels. Event Notifications are used to route the alert events to service destinations to automate response actions. |
| Audit alerts                                                                   | Provide a mechanism to identify and send notifications about issues that are found in audit logs.                                     | {{site.data.keyword.cloudaccesstraillong_notm}} \n * {{site.data.keyword.monitoringlong_notm}} \n * Event Notifications | {{site.data.keyword.cloudaccesstraillong_notm}} \n * {{site.data.keyword.monitoringlong_notm}} \n * Event Notifications | IBM Cloud Logs supports the configuration of alerts to detect audit issues and send notifications to targeted channels. Event Notifications are used to route the alert events to service destinations to automate response.                                                           |
| {: caption="Table 3. Architecture decisions for alerts" caption-side="bottom"}
