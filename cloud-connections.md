---

copyright:
  years: 2020

lastupdated: "2020-11-02"

keywords: Cloud connections, subnet, VPC, IBM cloud 

subcollection: power-iaas

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:preview: .preview}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}

# Cloud connections
{: cloud-connections}

Cloud connections provide an automated way to connect your {{site.data.keyword.powerSys_notm}} instances to the IBM Cloud resources that include classic and VPC network. Cloud connections create a Direct Link Connect (2.0) offering instance to connect your {{site.data.keyword.powerSys_notm}} instances to the IBM Cloud resources. The speed and reliability of the Direct Link connection extends the network of your organization data center and offers more consistent, higher-throughput connectivity, keeping traffic within the IBM Cloud network.

## Creating Cloud connections
{: create-cloud-connections}

If you are creating a new service, you will automatically receive two 5 Gbps Cloud connections at no cost. After the first two connections, charges apply based on speed and number of connections that you choose to have.
{: note}

To create a new Cloud connection, complete the following steps:

1. Sign into the **IBM Cloud Portal**.

2. Select the menu icon and select **Resource List**.

3. Click the arrow next to **Services**.

4. Select the Power Systems Virtual Server service you’d like to assign a Cloud connection.

5. Click **Cloud connections** in the left navigation pane, and click **Create new connection**.

6. Specify a connection name and select a connection speed. Default connection speed is 5 Gbps.

7. If you need access to other data centers outside your PowerVS region, you must select the global routing option. For example, you might use global routing to share workloads between dispersed IBM Cloud resources, such Dallas to Tokyo, or Dallas to Frankfurt.

8. Select **Endpoint destination** as follows to select the network connection to attach to the Direct Link gateway:

   a. **Classic Infrastructure**: Allows you to connect to IBM Cloud classic resources. Only one classic infrastructure connection is allowed per Direct Link gateway. You can also request a Generic Routing Encapsulation (GRE) tunnel configuration by specifying the GRE destination and GRE subnet IP addresses.  For more information, see [GRE tunneling](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-configuring-power#gre-tunneling).

   b. **VPC**:Allows you to connect to your account’s Virtual Private Cloud (VPC) resources. You must select the required VPC connection from the list of available connections.

9. Review the summary and click the check box to accept the terms and conditions.

10. Click **Create** to create a new Cloud connection.

## Configuring Cloud connections
{: configure-cloud-connections}

If you created a Power Systems Virtual Servers service that contains 2 default Cloud connections, you also have an initial subnet connected to those connections. You can view the attached subnets and add or remove subnets in the Cloud Connection details page. When you create or edit a subnet, you can also attach an existing Cloud connection. For information about adding a private network subnet, see[Configuring and adding a private network subnet](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-configuring-subnet).

Any changes to bandwidth might affect pricing.
{: note}

To configure Cloud connections, complete the following steps:

1. In the Power Systems Virtual Services dashboard, click **Cloud connections** in the left navigation pane.

2. Click the Cloud connection that you want to configure. The corresponding **Connection details** page appears.

    ![Cloud connection details](images/cloud-connections-details.png "Cloud connection details"){: caption="Figure 1. Cloud connection details" caption-side="bottom"}

3. Click **Edit details** icon.

    ![Edit details](images/edit-cloud-connection.png "Edit details"){: caption="Figure 2. Edit cloud connection details" caption-side="bottom"}

4. Make the required changes, review the pricing changes, and click **Save edits**.

## Attaching subnets to Cloud Connections
{: attach-subnet}

You must route Power Systems Virtual Server private network subnets over IBM Cloud™ Direct Link to allow connectivity between Power Systems Virtual Server instances and the IBM Cloud network.

The **Connection details** page contains the list of attached subnets.

![Attached subnets](images/attach-subnet.png "Attached subnets"){: caption="Figure 3. Attached subnets" caption-side="bottom"}

When you create a subnet or edit details of a subnet, you can attach an existing Cloud connection to the subnet. For steps to create a subnet, see [Configuring and adding a private network subnet](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-configuring-subnet).

##. Power Systems Virtual Server Network HA

## Configuring and adding a private network subnet

You can configure a private network subnet when you create an IBM® Power Systems Virtual Server instance. You must give your subnet a **Name** and specify a **Classless inter-domain routing (CIDR)**. When you specify a CIDR, the **Gateway**, **IP range**, and **DNS server** are automatically populated. You must use CIDR notation when you choose the IP ranges for your private network subnet. CIDR notation is defined in [RFC 1518](https://tools.ietf.org/html/rfc1518) and [RFC 1519](https://tools.ietf.org/html/rfc1519).

```
<IPv4 address>/<number>
```
{: codeblock}

The first IP address is always reserved for the gateway in all data centers. The second and third IP addresses are reserved for gateway high-availability (HA) in only the *WDC04* colo. The subnet address and subnet broadcast address are reserved in both colos.

To create a new subnet, complete the following steps:

1. Sign in to the [IBM Cloud Portal](https://cloud.ibm.com/).

2. Select the menu icon and select **Resource List**.

3. Click the arrow next to **Services**.

4. Select the Power Systems Virtual Server service you'd like to assign a subnet.

5. Click **Subnets** in the left navigation pane, then **Add subnet**.

    ![Configuring a subnet](images/Configuring-new-subnet.png "Configuring a subnet"){: caption="Figure 4. Configuring a subnet" caprtion-side="bottom"}

6. Optionally, select an existing cloud connection to which you want to attach this subnet. If you have set up another Cloud connection for redundancy purposes, you can select the second Cloud connection as well.

7. Click **Create subnet**.

A DNS server value of 9.9.9.9 might not be reachable if you don't have a public IP. This can cause the LPAR to hang during startup. Go with the default DNS server value of 127.0.0.1 to avoid this issue. As of now, you can add up to 20 DNS servers. The DNS IP addresses must be separated by commas.

You can also create and configure a private network subnet by using the IBM CLI. Use the following command to create a private network subnet:

```
ibmcloud pi network-create-private NETWORK_NAME --cidr-block CIDR --ip-range "startIP-endIP[,startIP-endIP]" [--dns-servers "DNS1 DNS2"] [--gateway GATEWAY] [--json]
```
{: codeblock}