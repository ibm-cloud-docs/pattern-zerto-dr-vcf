---
copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: <pattern-zerto-dr-vcf>

keywords:
---

{{site.data.keyword.attribute-definition-list}}

# Security design

{: \#security-design}

This section expands on the security aspect of the IBM Architecture framework in respect of the Zerto disaster recovery for VMware Cloud Foundation in IBM Cloud VPC pattern.

## Requirements

The requirements for the security aspect for the Zerto for disaster recovery for VMware workloads pattern focus on the following:

-   Provide encryption or privacy for the replication between the IBM Cloud regions.

## Considerations

| Security Areas                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Network Security**           | **Isolation:** Ensure proper network isolation for the Zerto infrastructure components, from unauthorized access. **Firewall Rules:** Implement strict firewall rules to control traffic between Zerto components and other systems within the IBM Cloud environment.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| **Access Control**             | **Role-Based Access Control (RBAC):** Implement RBAC to restrict access to Zerto components based on job responsibilities, ensuring that only authorized personnel can configure, manage. See [Permissions via Zerto Cloud Manager](https://help.zerto.com/bundle/Security.Hardening.HTML/page/Permissions_via_Zerto_Cloud_Manager.htm). **Authentication:** Enforce strong authentication mechanisms, such as multi-factor authentication (MFA), for accessing the Zerto management console and associated interfaces                                                                                                                                                                                                                     |
| **Data Encryption**            | In-Transit Encryption: Enable encryption for data in transit between Zerto components and VMware infrastructure to prevent interception and tampering. Use secure communication protocols like TLS. See[Network Encryption](https://help.zerto.com/bundle/Security.Hardening.HTML/page/Network_Encryption.htm) and [VRA to VRA Encryption](https://help.zerto.com/bundle/Security.Hardening.HTML/page/Virtual_Replication_Appliance.htm#vra_to_vra_encryption). **At-Rest Encryption:** Zerto does not support VM encryption, see [vSphere Features](https://help.zerto.com/bundle/Operability.Matrix.HTML/page/VMware_vSphere.htm). File encryption on a VM with applications such as BitLocker for non-boot disks is supported by Zerto. |
| **Vulnerability Management**   | **Regular Updates:** Keep Zerto software and underlying systems up-to-date with the latest security patches and updates to address potential vulnerabilities. **Scanning and Testing:** Regularly perform vulnerability assessments and penetration testing on the Zerto infrastructure to identify and remediate security weaknesses.                                                                                                                                                                                                                                                                                                                                                                                                     |
| **Compliance**                 | **Regulatory Compliance:** Ensure that the Zerto deployment within the IBM Cloud adheres to relevant industry regulations and compliance standards, such as GDPR, HIPAA, or any other applicable requirements.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **Data Residency and Privacy** | **Data Residency Policies:** Understand and comply with data residency requirements by configuring Zerto to align with IBM Cloud's data residency policies. **Privacy Considerations:** Address privacy concerns by implementing anonymization or pseudonymization of sensitive data within backups.                                                                                                                                                                                                                                                                                                                                                                                                                                       |

Table 1. Security design considerations
