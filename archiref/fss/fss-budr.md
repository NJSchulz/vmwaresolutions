---

copyright:

  years:  2020

lastupdated: "2020-07-16"

subcollection: vmwaresolutions


---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Business continuity
{: #fss-budr}

The IBM Cloud for VMware Regulated Workloads provides business continuity only at the management and edge layers.

## Management cluster
{: #fss-budr-management}

The management cluster relies upon native vSphere DRS capabilities to keep management services available to the platform administrators. Even with the configuration of the vSphere DRS features there are times when manual intervention is necessary to restore access to management services. The use of shared storage minimizes the necessity of manual restoration activities if an ESXi host is lost.

| System | Backup option | Frequency |
|---|---|---
|**Active Directory / DNS** | Image through the Veeam agent | Daily |
|**Veeam Virtual Machine** |  |  |
|**vCenter** | Backup server file| Daily |
|**NSX-T Controllers** | Backup server file | Daily|
|**vRealize Operations Manager** | VMDK through Veeam | Daily |
|**vRealize Log Insights** | VMDK through Veeam | Daily |
|**HyTrust Cloud Control** | Backup server file | |
|**HyTrust Data Control** | Backup server file | |
|**Virtual Machine Backup Server** | VMDK through Veeam| |
|**Juniper vSRX** | Backup server file through SCP from vSRX | Commit change |
{: caption="Table 1. Backup options" caption-side="top"}

### Veeam backup server
{: #fss-budr-management-veeam}

Veeam on IBM Cloud™ delivers reliable backup for virtual machines within the IBM Cloud for VMware Regulated Workloads environment.

Veeam is an available option that provides continuous backup of the management stack for protection against disasters. If corruption of any management stack component occurs, Veeam also provides rapid restoration to known good states. Veeam can also provide backup services for the workload cluster. The single site deployment must use the Veeam bare metal option to provide an acceptable backup repository.

The Veeam environment is provisioned initially with a single virtual machine. The ISV can extend this instance to their custom requirements, including remote database, more proxy servers, and scaling out of the backup repository.

Veeam is deployed to the management regions in both availability zones (AZ) for use in an MZR. Veeam in the primary (active) AZ is configured to use the secondary (standby) AZ vSAN as the backup repository. The management region in the secondary (standby) AZ of the MZR is backed up across to the primary AZ vSAN.

### Zerto service
{: #fss-budr-management-zerto}

Zerto provides disaster recovery and cloud mobility within a single, simple, scalable solution. Zerto Virtualization Manager (ZVM) and Virtual Replication Appliances (VRA) are an optional service that is deployed into the IBM Cloud for VMware Regulated Workloads environment. Zerto provides backup and disaster recovery into the cloud environment or between different zones within IBM Cloud.

Zerto VRAs are deployed in the management regions of the primary (active) and secondary (standby) AZs. Zerto is configured to synchronize from AZ1 to AZ2. If a DR event occurs AZ2 becomes the active region. Zerto uses the witness zone of the MZR to support the ZVM.

![IBM Cloud for VMware Regulated Workloads Zerto](../../images/fss-zerto-topology.svg "Zerto topology"){: caption="Figure 1. Zerto topology" caption-side="bottom"}

## Edge cluster
{: #fss-budr-edge}

The edge cluster does not use any vSphere resiliency features and backup of the gateway VMs is not performed. The gateway appliances deliver resilience at the application layer through formation of a high-availability cluster at the time of deployment. When you are using the vSRX, it is recommended that the rescue configuration is set anytime a change is made to the running configuration. To update it, in operational mode, run `request system configuration rescue save`. The use of an SCP server to automatically back up the configuration anytime changes are made is optional and left to the client to implement.

```
vSRX config archive on commit

system {
  archival {
    configuration {
      transfer-on-commit;
      archive-sites {
        scp://username@host:<port>url-path password password;
      }
    }
  }
}

[edit system archival configuration]
archive-sites {
  ftp://username@host:<port>url-path password password;
  scp://username@host:<port>url-path password password;
  file://<path>/<filename>;
  http://username@host: url-path password password;
}

```

## Workload cluster
{: #fss-budr-workload}

Business continuity at the workload layers is the responsibility of the client to design, implement, and maintain.

There are several options available in the IBM Cloud including:
* Veeam backup server
* Zerto service

**Next topic**: [HyTrust integration](/docs/vmwaresolutions?topic=vmwaresolutions-fss-hytrust)

## Related links
{: #fss-budr-related}

* [IBM Cloud compliance programs](https://www.ibm.com/cloud/compliance)
* [Veeam](/docs/vmwaresolutions?topic=vmwaresolutions-veeam_considerations)
* [vSRX archival configuration](https://www.juniper.net/documentation/en_US/junos/topics/task/configuration/junos-software-system-management-router-configuration-archiving.html#id-10944516){:external}