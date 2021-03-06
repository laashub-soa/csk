# Use container-optimized Alibaba Cloud Linux 2

Container Service for Kubernetes \(ACK\) allows you to create nodes that run the Alibaba Cloud Linux 2 operating system and provides optimizations for multiple scenarios based on the high-performance kernel of Alibaba Cloud Linux 2. This topic describes the benefits of using Alibaba Cloud Linux 2 in ACK clusters and the optimizations on Alibaba Cloud Linux 2 that are provided by ACK to meet the requirements in different scenarios.

Alibaba Cloud Linux 2 is a next-generation proprietary Linux distribution that is developed by Alibaba Cloud. It provides a safe, stable, and high-performance customized environment for applications on Elastic Compute Service \(ECS\) instances. Alibaba Cloud Linux 2 is optimized for the cloud infrastructure and aims to deliver a better runtime experience. You can create an instance from an Alibaba Cloud Linux 2 image. Alibaba Cloud Linux 2 images are free of charge. Alibaba Cloud provides long-term technical support for Alibaba Cloud Linux 2 images.

## Benefits of Alibaba Cloud Linux 2 images

Alibaba Cloud Linux 2 is developed for use on the Alibaba Cloud IaaS platform and provides various features specific to the Alibaba Cloud environment. Alibaba Cloud Linux 2 provides the following features:

-   The Linux distribution with the fastest boot time on Alibaba Cloud.
-   Deeply optimized for large-scale virtual machines and bare metal servers. This is particularly suitable for multi-task scenarios that involve large-sized instances.
-   Pre-installed with commonly used software such as Alibaba Cloud CLI and cloud-init. This reduces the costs of cloud resource management.
-   A streamlined kernel that minimizes the attack surface and system resource occupation.
-   An optimized technical support system that offers multi-channel technical support on Alibaba Cloud.
-   Fixes vulnerabilities at the earliest opportunity based on Common Vulnerabilities and Exposures \(CVE\).
-   Supports live patching of the kernel. This ensures service continuity when vulnerabilities are being fixed.

Alibaba Cloud Linux 2 provides the following benefits for the performance of applications that run Alibaba Cloud Linux 2:

-   Significantly reduces system boot time of ECS instances. Quickly scales out computing resources when system overloading is detected. Reduces the system boot time by 29% compared with CentOS 7.
-   Provides optimizations for ECS instances in multi-task scenarios and improves the performance of large-sized instances by 16%.
-   Improves the efficiency of system scheduling by 11%.
-   Optimizes the Linux networking stack to improve the network performance by 7.8% compared with CentOS 7.
-   By default, Alibaba Cloud Linux 2 has compiled the Bottleneck Bandwidth and Round-trip Propagation Time \(BBR\) congestion control algorithm. In scenarios where frequent Internet access is required, you can change the congestion control algorithm of containers to BBR to improve the bandwidth stability for Internet access.
-   Optimizes the TLS encryption protocol.
-   Supports the Budget Fair Queueing \(BFQ\) I/O scheduler to reduce disk latency.

## Optimizations provided by ACK on Alibaba Cloud Linux 2 in different scenarios

ACK provides kernel optimizations to allow containerized workloads to support more densely deployed container tasks without service interruptions. Based on the optimizations provided by Alibaba Cloud Linux 2 and its kernel, ACK offers further optimizations to enhance the speed and stability of containerized workloads in various scenarios.

-   Container network optimization

    Alibaba Cloud Linux 2 is equipped with kernel 4.19. The Terway network plug-in supports the IPVLAN driver, which improves the network performance in short-packet communications by 40% compared with the traditional bridge interface and policy-based routing \(PBR\). By default, Alibaba Cloud Linux 2 has compiled the Bottleneck Bandwidth and Round-trip Propagation Time \(BBR\) congestion control algorithm. In scenarios where frequent Internet access is required, you can change the congestion control algorithm of containers to BBR to improve the bandwidth stability of Internet access. This significantly improves the network stability of the Internet access for containers and the network stability when you pull images over the Internet.

-   Support for Sandboxed-Container

    Alibaba Cloud has established partnerships with the Kata Containers and Clear Linux communities. You can seamlessly deploy the Kata Containers solution on ECS Bare Metal instances. ACK also reduces the boot time of RunV containers. This ensures that the Kata Containers solution can work as expected. ACK also provides clusters that run sandboxed containers. This type of cluster offers almost the same user experience as normal clusters. This allows you to run applications in lightweight sandboxed environments, which can be used to isolate workloads among multiple tenants and isolate untrusted applications. Sandboxed-Container enhances security and has little impact on application performance.

-   Node AutoScaler optimization

    Alibaba Cloud Linux 2 significantly reduces the system boot time of ECS instances. The average system boot time is reduced by 29% compared with CentOS 7. Based on the auto scaling feature provided by ACK, ACK clusters can quickly create and launch ECS instance-based nodes when system overload is detected. Then, ACK schedules and starts application instances accordingly. Alibaba Cloud Linux 2 enables quick scale-out and startup of cluster nodes to meet unexpected traffic spikes at the earliest opportunity.

-   CoreDNS optimization

    When pods are densely deployed in an ACK cluster, Domain Name Service \(DNS\) resolution may fail because UDP connections account for excessive entries in the conntrack table. Alibaba Cloud Linux 2 has optimized the conntrack table to reduce table entries by 50% for the same DNS query rate. This significantly improves the success rate of DNS queries.

-   More fine-grained resource monitoring and control

    The kernel of Alibaba Cloud Linux 2 provides fine-grained visualization and control capabilities for containers, such as PSI pressure metrics, per-cgroup kswapd, and memory priority. In ACK clusters that run Alibaba Cloud Linux 2, you can use CGroup Controllers to utilize these capabilities and implement fine-grained configurations and dynamic updates of resources such as BufferIO Control, TCP, CPUSet, Mem, and NUMA. This provides a gradual improvement in resource utilization and minimizes the interference between applications.

-   AI task and data processing acceleration

    Alibaba Cloud Linux 2 optimizes large-scale instances to speed up the processing of high-performance computing tasks and refines stream read and write methods to enhance large file read and write performance. This significantly improves the efficiency of processing AI and high-performance computing tasks. The following data is recorded in test environments:

    -   Test: Use Alluxio to load 1,152 files that add up to 144 GB of Object Storage Service \(OSS\) data through 64 threads. CentOS requires 3 minutes and 25 seconds while Alibaba Cloud Linux 2 requires only 2 minutes and 19.037 seconds. Alibaba Cloud Linux 2 provides a speed that is 1.6 times that provided by CentOS.
    -   Test: Train the ResNet50 model with a batch size of 128 and cache data to Alluxio. In CentOS with NVIDIA V100, the speed is 5,212.00 images/s. In Alibaba Cloud Linux 2 with NVIDIA V100, the speed is 8,746.59 images/s. Alibaba Cloud Linux 2 provides a speed that is 1.7 times that provided by CentOS.
-   Others
    -   Based on Linux kernel 4.19, ACK integrates Alibaba Cloud Linux 2 with core capabilities and containerization best practices of Alibaba Group.
    -   Reduces the performance loss of OverlayFS and minimizes the storage performance decrease caused by containerization.
    -   In most cases, sysctls are namespaced. In Kernel 4.19, most sysctls can be independently set on pods. This allows you to set different TCP timeout values and retransmit timeout values for different applications. You cannot modify these parameters in CentOS 7. In Alibaba Cloud Linux 2, you can set these parameters on pods.

## How to set the operating system image of a node to Alibaba Cloud Linux 2

When you create an ACK cluster in the ACK console, set **Operating System** to **Alibaba Cloud Linux 2.1903**. This way, you can use Alibaba Cloud Linux 2 as the operating system image of the nodes in the cluster. For more information, see [Create a dedicated Kubernetes cluster](/intl.en-US/User Guide for Kubernetes Clusters/Cluster management/Create Kubernetes clusters/Create a dedicated Kubernetes cluster.md).

![linux](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0635359951/p96758.png)

**Note:** After you select Alibaba Cloud Linux 2, ACK automatically checks for security patches of Alibaba Cloud Linux 2 and installs the patches during cluster creation, cluster expansion, and node autoscaling.

**Related topics**  


[Product page of Alibaba Cloud Linux 2](https://www.alibabacloud.com/zh/products/alinux)

[Alibaba Cloud Kernel official website](https://alibaba.github.io/cloud-kernel/)

