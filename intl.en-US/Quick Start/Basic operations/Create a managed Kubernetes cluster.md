---
keyword: [Kubernetes, K8s, create]
---

# Create a managed Kubernetes cluster

This topic describes how to create a managed Kubernetes cluster in the Container Service for Kubernetes \(ACK\) console.

-   Before you start, you must authorize ACK to access other cloud services. For more information, see [Use ACK for the first time](/intl.en-US/Quick Start/Use ACK for the first time.md).
-   Log on to the [Resource Access Management \(RAM\) console](https://ram.console.aliyun.com/) to activate the relevant cloud services.
-   [Auto Scaling \(ESS\) is activated]().

The following example shows how to create a managed Kubernetes cluster. Default values and minimal configurations are used in certain settings.

For more information about the limits of ACK clusters, see [Create a managed Kubernetes cluster](/intl.en-US/User Guide for Kubernetes Clusters/Cluster management/Create Kubernetes clusters/Create a managed Kubernetes cluster.md).

1.  Log on to the [ACK console](https://cs.console.aliyun.com).

2.  In the left-side navigation pane, click **Clusters**.

3.  In the upper-right corner of the Clusters page, click **Create Kubernetes Cluster**. In the Select Cluster Template dialog box, find Standard Managed Cluster and click **Create**.

4.  Set the cluster parameters.

    Default values are used for most parameters in this example. The following table describes the parameters.

    1.  Complete the **Cluster Configurations** step.

        |Parameter|Description|
        |---------|-----------|
        |**Cluster Name**|Enter a name for the ACK cluster.

**Note:** The name must be 1 to 63 characters in length. It can contain digits, letters, and hyphens \(-\). |
        |**Cluster specifications**|Select a cluster type. **Standard edition** and **Professional** are supported. |
        |**Region**|Select a region to deploy the ACK cluster. |
        |**Resource Group**|Move the pointer over **All Resources** at the top of the page and select the resource group to which the ACK cluster belongs. The name of the selected resource group appears on the page.

![Resource Groups](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9688404061/p127165.png)

**All Resources** is selected in this example. |
        |**Time Zone**|Select a time zone for the ACK cluster By default, the time zone configured for your browser is selected. |
        |**Kubernetes Version**|Use the default setting. |
        |Container Runtime

|By default, the Docker runtime is selected. Use the default setting. For more information about the differences between Docker and Sandboxed-Container, see [How do I select between Docker and Sandboxed-Container?](/intl.en-US/User Guide for Kubernetes Clusters/Sandboxed-Container management/How do I select between Docker and Sandboxed-Container?.md). |
        |**VPC**|Set a virtual private cloud \(VPC\) to deploy the cluster.

Select a virtual private cloud \(VPC\) to deploy the ACK cluster. Shared VPCs and standard VPCs are supported.

        -   Shared VPC: The owner of a VPC \(resource owner\) can share vSwitches in the VPC under the account of the owner with other accounts in the same organization.
        -   Standard VPC: The owner of a VPC \(resource owner\) cannot share vSwitches in the VPC under the account of the owner with other accounts.
**Note:** ACK clusters support only VPCs. You can select a VPC from the drop-down list. If no VPC is available, click **Create VPC** to create one. For more information, see [Create a VPC](/intl.en-US/VPCs and VSwitches/VPC management/Create a VPC.md). |
        |**VSwitch**|Select vSwitches.

You can select up to three vSwitches that are deployed in different **zones**. If no vSwitch is available, click **Create VSwitch** to create one. For more information, see [Create a VSwitch](/intl.en-US/VPCs and VSwitches/VSwitch management/Create a VSwitch.md). |
        |**Network Plug-in**|By default, Flannel is selected. For more information about Flannel and Terway, see [Flannel and Terway](/intl.en-US/User Guide for Kubernetes Clusters/Network management/Use Terway.md). |
        |**Pod CIDR Block**|If you select **Flannel**, you must set **Pod CIDR Block**.

The CIDR block specified by **Pod CIDR Block** cannot overlap with that of the VPC or existing ACK clusters in the VPC. The CIDR Block cannot be modified after it is specified. The Service CIDR block cannot overlap with the Pod CIDR block. For more information about subnetting for ACK clusters, see [Assign CIDR blocks to resources in a Kubernetes cluster under a VPC](/intl.en-US/User Guide for Kubernetes Clusters/Network management/Assign CIDR blocks to resources in a Kubernetes cluster under a VPC.md). |
        |**Service CIDR**|Set **Service CIDR**. The CIDR block specified by **Service CIDR** cannot overlap with that of the VPC or existing ACK clusters in the VPC. The CIDR block cannot be modified after it is specified. The Service CIDR block cannot overlap with the pod CIDR block. For more information about subnetting for ACK clusters, see [Assign CIDR blocks to resources in a Kubernetes cluster under a VPC](/intl.en-US/User Guide for Kubernetes Clusters/Network management/Assign CIDR blocks to resources in a Kubernetes cluster under a VPC.md). |
        |**IP Addresses per Node**|The default value is 64. **IP Addresses per Node** specifies the maximum number of IP addresses that can be assigned to each node. |
        |**Configure SNAT**|By default, **Configure SNAT for VPC** is selected.

By default, an ACK cluster cannot be accessed over the Internet. If the VPC that you select for the ACK cluster cannot access the Internet, you can select **Configure SNAT for VPC**. Then, ACK creates a Network Address Translation \(NAT\) gateway and configures Source Network Address Translation \(SNAT\) entries to enable Internet access for the VPC. |
        |**Public Access**|By default, **Expose API Server with EIP** is not selected.

The ACK API server provides multiple HTTP-based RESTful APIs, which can be used to create, delete, modify, query, and monitor resources such as pods and Services.

        -   If you select this check box, an EIP is created and attached to a public-facing Server Load Balancer \(SLB\) instance. Port 6443 used by the API server is opened on master nodes. You can connect to and manage the ACK cluster by using kubeconfig over the Internet.
        -   If you clear this check box, no EIP is created. You can connect to and manage the ACK cluster only by using kubeconfig from within the VPC. |
        |**RDS Whitelist**|Set the Relational Database Service \(RDS\) whitelist. Add the IP addresses of nodes in the ACK cluster to the RDS whitelist.

**Note:** To enable an RDS instance to access the ACK cluster, you must deploy the RDS instance in the same VPC as the ACK cluster.

By default, Relational Database Service \(RDS\) whitelist is not configured. |
        |**Security Group**|You can select **Create Basic Security Group**, **Create Advanced Security Group**, or **Select Existing Security Group**. For more information, see [Overview](/intl.en-US/Security/Security groups/Overview.md).

By default, **Create Advanced Security Group** is selected. The default setting is used in this example. |
        |Advanced settings

|The advanced settings of the cluster are not configured in this example. For more information about the advanced settings of an ACK cluster, see [Configure advanced settings for a cluster](/intl.en-US/User Guide for Kubernetes Clusters/Cluster management/Create Kubernetes clusters/Create a managed Kubernetes cluster.md).|

    2.  Click **Next:Worker Configurations** to configure worker nodes in the **Worker Configurations** step.

        |Parameter|Description|
        |---------|-----------|
        |Worker Instance

|Specify whether to use existing Elastic Compute Service \(ECS\) instances or purchase ECS instances. By default, **Create Instance** is selected. |
        |**Billing Method**|By default, the **pay-as-you-go** billing method is selected. The **pay-as-you-go** and **subscription** billing methods are supported. |
        |**Instance Type**|You can select multiple instance types. For more information, see [Instance families](/intl.en-US/Instance/Instance families.md). |
        |**Selected Types**|The selected instance types are displayed. |
        |**Quantity**|Specify the number of worker nodes to be created. |
        |**System Disk**|By default, **ultra disk** is selected. The default storage capacity is **120 GiB**.|
        |**Mount Data Disk**|By default, this option is not selected.

**Enhanced SSDs**, **SSDs**, and **ultra disks** are supported. You can enable disk **encryption** and **backup** when you mount data disks. |
        |**Operating System**|By default, Alibaba Cloud Linux 2.1903 is selected.|
        |**Logon Type**|        -   Set the key pair.
            -   **Key Pair**: Select an SSH key pair from the drop-down list.
            -   **create a key pair**: Create an SSH key pair if no SSH key pair is available. For more information about how to create an SSH key pair, see [Create an SSH key pair](/intl.en-US/Security/Key pairs/Use an SSH key pair/Create an SSH key pair.md). After the key pair is created, set it as the credential that is used to log on to the ACK cluster.
        -   Password:
            -   **Password**: Enter the password that is used to log on to the nodes.
            -   **Confirm Password**: Enter the password again. |
        |Advanced settings of worker nodes

|The advanced settings of worker nodes are not configured in this example. For more information about the advanced settings of a cluster, see [Configure advanced settings for a cluster](/intl.en-US/User Guide for Kubernetes Clusters/Cluster management/Create Kubernetes clusters/Create a managed Kubernetes cluster.md).|

    3.  Click **Next:Component Configurations** to configure components in the **Component Configurations** step.

        |Parameter|Description|
        |---------|-----------|
        |**Ingress**|By default, **Install Ingress Controllers** is selected. Select **Public Network** for **SLB Network Type**. |
        |**Volume Plug-in**|By default, **CSI** is selected. For more information about the FlexVolume and CSI plug-ins, see [Volume plug-ins](/intl.en-US/User Guide for Kubernetes Clusters/Storage management-Flexvolume/Volume plug-ins.md). |
        |**Monitoring Agents**|Specify whether to install the Cloud Monitor agent. By default, **Install CloudMonitor Agent on ECS Instance** and **Enable Prometheus Monitoring** are selected. After the Cloud Monitor agent is installed on ECS nodes, you can view monitoring data about the nodes in the Cloud Monitor console.

The Cloud Monitor Agent is not installed in this example. |
        |**Log Service**|Specify whether to enable Log Service. You can select an existing Log Service project or create a new one.

By default, **Enable Log Service** is selected. When you create an application, you can perform a few steps to enable Log Service. For more information, see [Use Log Service to collect container logs](/intl.en-US/User Guide for Kubernetes Clusters/Log management/Use Log Service to collect container logs.md).

By default, **Install node-problem-detector and Create Event Center** is selected. You can also specify whether to **create Ingress dashboard** in the Log Service console.

Log Service is not set up in this example. |
        |**Workflow Engine**|Specify whether to enable Alibaba Cloud Genomics Compute Service \(AGS\).

        -   If you select this check box, the system automatically installs the AGS workflow plug-in when the system creates the ACK cluster.
        -   If you clear this check box, you must manually install the AGS workflow plug-in. For more information, see [Introduction to AGS CLI](/intl.en-US/User Guide for Genomics Service/AGS workflow/Introduction to AGS CLI.md).
**AGS** is not selected in this example. |

    4.  Click **Next:Confirm Order**.

    5.  Read the **Terms of Service** and select the check box, and click **Create Cluster**.

        **Note:** It requires about 10 minutes to create an ACK cluster that contains multiple nodes.


