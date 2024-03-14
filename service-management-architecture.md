---
copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-zerto-dr-vcf

keywords:
---
# Architecture decisions for service management
{: #service}

The following sections summarize the architecture decisions for service management for the Zerto for VCF resiliency pattern.

## Architecture decisions for monitoring
{: #monitoring}

### Architecture decisions for monitoring
{: #architecture decisions for monitoring}

| **Architecture decision**                                                    | **Requirement**                                                                                    | **Options**                                                | **Decision**                    | **Rationale**                                                                                                                                                                                                                                      |
| ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Operational monitoring of cloud infrastructure and services                        | Monitor system health to detect issues that might impact the availability of the system and application. | Zerto Analytics, IBM Cloud Monitoring, 3rd party monitoring tool | Zerto Analytics                       | Zerto Analytics is included in the service (requires the creation of a Zerto account).                                                                                                                                                                   |
| Operational monitoring of applications                                             | Monitor app health to detect issues that might impact the availability of the app.                       | IBM Cloud Monitoring, Instana (SaaS), 3rd party monitoring tool  | IBM Cloud Monitoring + Instana (SaaS) | Instana is used along with IBM Cloud Monitoring to get more application performance metrics and automate application performance management. Instana provides data and actionable insights to monitor the applications and automate root-cause analysis. |
{: caption="Table 1. Architecture decisions for monitoring" caption-side="bottom"}

### Logging Architecture decision
{: #logging}

| **Architecture decision**                                              | **Requirement**                                                                                         | **Options**                                                   | **Decision**                           | **Rationale**                                                                                                                                         |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- | -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Log monitoring of cloud infrastructure and services                          | Monitor operational logs to detect issues that might impact the availability of the system and application.   | IBM Cloud Logging, VMware Aria, 3rd party logging tool              | IBM Cloud Logging                            | IBM Cloud Logging collects operational logs from applications, platform resources, and infrastructure and provides interfaces to view and analyze all logs. |
| Log monitoring of applications                                               | Monitor application operational logs to detect issues that might impact the availability of the applications. | IBM Cloud Logging, application logging tool, 3rd party logging tool | IBM Cloud Logging + application logging tool | Use the application logging tool to send application logs to IBM Cloud Logging and aggregate application-specific log details.                              |
| Log monitoring of databases                                                  | Monitor database logs to detect issues that might impact the availability of the database.                    | IBM Cloud Logging, database logging tools, 3rd party logging tool   | IBM Cloud Logging + database logging tool    | Use the database logging tools along with IBM Cloud Logging to get more database specific log information.                                                  |
{: caption="Table 2. Architecture decisions for logs" caption-side="bottom"}

### Alerting Architecture decision
{: #alerting}

| **Architecture decision**                                                | **Requirement**                                                                                                                 | **Options**                                                 | **Decision**                                                | **Rationale**                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Operational alerts                                                             | Provide a mechanism to identify and send notifications about operational issues that are found across application and infrastructure. | IBM Cloud Monitoring + IBM Cloud Logging + Event Notifications    | IBM Cloud Monitoring + IBM Cloud Logging + Event Notifications    | IBM Cloud Monitoring and IBM Cloud Logging support the configuration of alerts to detect operational issues and send notifications to targeted channels. Event Notifications are used to route the alert events to service destinations to automate response actions. |
| Audit alerts                                                                   | Provide a mechanism to identify and send notifications about issues that are found in audit logs.                                     | IBM Cloud Activity Tracker + IBM Monitoring + Event Notifications | IBM Cloud Activity Tracker + IBM Monitoring + Event Notifications | IBM Activity Tracker supports the configuration of alerts to detect audit issues and send notifications to targeted channels. Event Notifications are used to route the alert events to service destinations to automate response a                                   |
{: caption="Table 3. Architecture decisions for alerts" caption-side="bottom"}
