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

# Underlay networking
{: #fss-underlay-network}

The IBM Cloud for VMware Regulated Workloads requires isolated networking between the workload clusters and the management and edge services clusters.

## Management cluster
{: #fss-underlay-network-management}

The management cluster requires two VLANs to support the management functions.

One VLAN includes subnets for ESXi management (vmk0) and management services such as the vCenter. The gateway appliance establishes security zones and policies to control the flow of traffic within the VLAN between the subnets. Unsolicited traffic from the ESXi or bare metal hosts is prevented from reaching the management systems. The perimeter gateway controls the traffic flow from these two subnets to any other area outside of the management region.

The second VLAN includes subnets that are dedicated to vMotion and vSAN. No routing between these subnets is allowed and the VLAN is isolated by the perimeter gateway from all other security zones and networks.

## Edge cluster
{: #fss-underlay-network-edge}

The optional edge services cluster adds two transit network VLANs to the solution. These VLANs connect the vSRX to both the back-end customer routers (BCR) for private traffic and FCR for public (internet) traffic flows. When a private-only deployment is wanted, the public transit VLAN is not ordered. The management VLAN is trunked to the edge services cluster. The network design is executed in this manner to enable the vSRX to control traffic flows within the management zone and between the management zone and the IBM Cloud private and public networks. The FortiGate appliance transit and VLAN network design is the same as the one used with the edge services cluster.

The vSRX running on the edge cluster connects the management network to the private and public transit networks. The vSRX is configured to allow only traffic in or out of the management region that is necessary for proper operation and monitoring of the environment. The vSRX also isolates all traffic between the clusters' ESXi hosts and the vCenter. ESXi hosts within a cluster can communicate with each other and the vCenter but ESXi hosts in one cluster (workload or management for example) are unable to communicate with the hosts of any other clusters. The limitation of cross cluster traffic is enforced by the vSRX and the configuration of the ESXi hosts' own firewalls.

The edge cluster is the peering point for traffic between the ISV on-premises and the IBM Cloud for VMware Regulated Workloads and it also serves as the demarcation for traffic from the bank. The ISV uses the vSRX as the secure tunnel endpoint for its VPN.

Traffic from the ISV's customer passes through the vSRX in an encrypted tunnel, which ends on the overlay network virtual edge device.

## Workload cluster
{: #fss-underlay-network-workload}

The workload cluster network design is closely aligned to that of a traditional vCenter Server deployment. VLANs and subnets are provisioned to support vMotion, vSAN, VTEPS for the Software-Defined Networking (SDN) network, and workload cluster host management functions.

Within the workload clusters, NSX-T provides a highly secure and flexible software defined network to support the application requirements. NSX-T management is external to the workload cluster thus ensuring that network and security changes are not possible by anyone other than the designated administrators. All north-south network access in the workload cluster is done through private and secure connections by using IPsec or IBM Direct Link. The workload clusters are protected by the same edge services cluster with the vSRX or the physical FortiGate that protects the management plane.

## IBM Cloud networking
{: #fss-underlay-network-cloud}

The physical network of IBM Cloud is separated into two distinct networks: public and private. The private network also contains the management Intelligent Platform Management Interface (IPMI) traffic to the physical servers.

![IBM Cloud high–level network](../../images/fss-ibmcloudnetwork.svg "IBM Cloud high–level network"){: caption="Figure 1. IBM Cloud high–level network" caption-side="bottom"}

### Public network
{: #fss-underlay-network-cloud-public}

IBM Cloud data centers and network points of presence (PoPs) have multiple 1 Gbps or 10-Gbps connections to the top-tier transit and peering network carriers. Network traffic from anywhere in the world connects to the closest network PoP, and travels directly across the network to its data center, minimizing the number of network hops and handoffs between providers.

Inside the data center, IBM Cloud provides 1 Gbps or 10 Gbps of network bandwidth to individual servers through a pair of separate, peer-aggregated front-end customer switches (FCS). These aggregated switches are attached to a pair of separate routers, FCR, for L3 networking.

This multitier design allows the network to scale across racks, rows, and pods within an IBM Cloud Data Center.

### Private network
{: #fss-underlay-network-cloud-private}

All IBM Cloud data centers and PoPs are connected by the private network backbone. This private network is separate from the public network, and it enables connectivity to services in IBM Cloud data centers around the world. Moving data between IBM Cloud data centers is done through multiple 10 Gbps or 40 Gbps connections to the private network.

Similar to the public network, the private network is multi-tiered in that servers and other infrastructure components are connected to aggregated back-end customer switches (BCS). These aggregated switches are attached to a pair of separate back-end customer routers (BCR) for L3 networking. The private network also supports the ability to use jumbo frames (MTU 9000) for physical host connections.

### Management network
{: #fss-underlay-network-cloud-management}

In addition to the public and private networks, each IBM Cloud server is connected for management to the private primary network subnet. This connection allows Intelligent Platform Management Interface (IPMI) access to the server independently of its CPU, firmware, and operating system, for maintenance and administration purposes.

### Primary and portable IP blocks
{: #fss-underlay-network-cloud-ipblocks}

IBM Cloud allocates two types of IP addresses to be used within the IBM Cloud infrastructure:

* Primary IP addresses are assigned to devices, Bare Metal, and virtual servers that are provisioned by IBM Cloud. Do not manually assign any IP addresses in these blocks.
* Portable IP addresses are provided for you to assign and manage as needed. The IBM Cloud for VMware Regulated Workloads automation provisions several portable IP ranges for its use. Use only the portable IP address ranges that are assigned to specific NSX-T components and specified for customer use.

Primary or portable IP addresses can be made routable to any VLAN within your account when the account is configured as a **Virtual Routing and Forwarding (VRF)** account.

### Virtual routing and forwarding
{: #fss-underlay-network-cloud-vrf}

The IBM Cloud infrastructure customer portal account must be configured as a Virtual Routing and Forwarding (VRF) account, which will enable automatic global routing between subnet IP blocks. All accounts with Direct-Link connections must be converted to, or created as, a VRF account.

As various connectivity options along with network routing options require that the IBM Cloud account is in a VRF mode, it is recommended that the account is in VRF mode before provisioning the IBM Cloud for VMware Regulated Workloads.

### Physical host connections
{: #fss-underlay-network-cloud-hosts}

Each physical host in this design has two redundant pairs of 10 Gbps Ethernet connections into each IBM Cloud Top of Rack (ToR) switch (public and private). The adapters are set up as individual connections (unbonded) for a total of 4 x 10 Gbps connections. This allows networking interface card (NIC) connections to work independently from each other.

Removing physical network connectivity to the public or private network for the Bare Metal Servers that are used within the vCenter Server offering is not possible. Physical ports on the internal NIC of the bare metal can be disabled, but there is no support for unplugging the cables. This configuration is some times referred to as "Air-Gapped" which is short hand for those actions necessary to ensure that the public side network ports of the ESXi hosts are disabled, that the ToR ports for those connections are disabled, and that IBM Cloud IAM is configured to prevent those without sufficient privileges from taking any action to enable the connections. Additionally, the public client-side VLAN is assigned to the perimeter gateway device and secured in such a way as to prevent any traffic to/from the public VLAN though the gateway and the gateway connections to the public transit VLAN are also administratively down (as opposed to disconnected) enabling monitoring for any attempt of traffic egressing/ingressing across the public transit VLAN to/from the FCR.

While IBM Cloud does offer an SSL VPN option, the use of the SSL VPN is discouraged and use should be strictly limited to situations where out of band access to the IBM Cloud for VMware Regulated Workloads is essential.

![Physical host connections](../../images/fss-nics-physical.svg "Physical host connections"){: caption="Figure 2. Physical host connections" caption-side="bottom"}

### VLANs and underlay to overlay routing
{: #fss-underlay-network-cloud-vlans}

The IBM Cloud for VMware Solutions offerings are designed with 3 VLANs, one public and two private, assigned upon deployment. As shown in the previous figure, the public VLAN is assigned to `eth1` and `eth3`, and the private VLANs are assigned to `eth0` and `eth2`.

The public and the first private VLAN created and assigned in this design are untagged by default within the IBM Cloud. Then, the additional private VLAN is trunked on the physical switch ports and tagged within the VMware port groups that are using these subnets.

![VLAN connections](../../images/fss-nics-vlans.svg "VLAN connections"){: caption="Figure 3. VLAN connections" caption-side="bottom"}

The private network consists of two VLANs within this design. Three subnets are allocated to the first of these VLANs (here designated Private VLAN A):

* The first subnet is a primary private IP subnet range that IBM Cloud assigns to the physical hosts.
* The second subnet is used for management virtual machines such as vCenter Server Appliance and Platform Services Controller
* The third subnet is used for the encapsulated overlay network Tunnel Endpoints (VTEPs) assigned to each host through the NSX Manager.

In addition to Private VLAN A, a second private VLAN (here designated Private VLAN B) exists to support VMware features such as vSAN and vMotion. As such, the VLAN is divided into two or more portable subnets:

* The first subnet is assigned to a kernel port group for vMotion traffic.
* The remaining subnet or subnets are used for storage traffic:
  * When using vSAN, a subnet is assigned to kernel port groups that are used for vSAN traffic.

The public network consists of one VLAN within this design. Subnets allocated to the VLAN:

* The first subnet is a Primary Public IP subnet range that IBM cloud assigns to the physical hosts
  * The hosts are assigned a public IP address but this IP address is not configured on the hosts, so they are not directly accessible on the public network.
* The second subnet is used for public access of components like a virtual gateway appliance.
  * The public VLAN is intended to provide public internet access.

All subnets that are configured as part of an IBM Cloud for VMware Regulated Workloads automated deployment use IBM Cloud managed ranges. This is to ensure that any IP address can be routed to any data center within the IBM Cloud.

Review the following table for a summary.

| VLAN | Type | Description |
|--------- |---- |----------- |
| Public C | Primary  | Assigned to physical hosts for public network access.  |
| Private A | Primary  | Single subnet assigned to physical hosts assigned by IBM Cloud. Used by the management interface for vSphere management traffic. |
| Private A | Portable | Single subnet that is assigned to virtual machines that function as management components |
| Private A | Portable | Single subnet that is assigned to NSX-V or NSX-T VTEP |
| Private B | Portable | Single subnet that is assigned for vSAN, if in use |
| Private B | Portable | Single subnet assigned for NAS, if in use |
| Private B | Portable | Single subnet assigned for vMotion |
{: caption="Table 1. VLAN and subnet summary" caption-side="top"}

In this design, all VLAN-backed hosts and virtual machines are configured to point to the perimeter gateway as the default route. While the IBM Cloud for VMware Regulated Workloads instances enable the use of SDN, network overlays created within a VMware instance that include routing to internal subnets are not known by the perimeter gateway unless dynamic routing protocols or static routes are configured.

The private network connections are configured to use a jumbo frame MTU size of 9000 to improve performance for large data transfers, such as storage and vMotion. This is the maximum MTU that is allowed within VMware and by IBM Cloud. The public network connections use a standard ethernet MTU of 1500. This must be maintained as any changes might cause packet fragmentation over the internet.

**Next topic**: [Overlay networking](/docs/vmwaresolutions?topic=vmwaresolutions-fss-overlay-network)

## Related links
{: #fss-underlay-network-related}

* [IBM Cloud compliance programs](https://www.ibm.com/cloud/compliance)