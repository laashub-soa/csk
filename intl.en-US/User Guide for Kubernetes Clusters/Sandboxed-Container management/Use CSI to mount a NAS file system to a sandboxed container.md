---
keyword: [how to directly mount a NAS file system to a sandboxed container, CSI, improve the I/O performance]
---

# Use CSI to mount a NAS file system to a sandboxed container

You can use the CSI plug-in to mount a Network Attached Storage \(NAS\) file system to a sandboxed container. This significantly improves I/O performance. This topic provides an example of how to use the CSI plug-in to directly mount a NAS file system to a sandboxed container. A NAS file system can also be mounted to a container through the host or the 9pfs file system. This topic also compares the I/O performance in these mount modes.

-   [Create a managed ACK cluster that supports sandboxed containers](/intl.en-US/User Guide for Kubernetes Clusters/Sandboxed-Container management/Create a managed ACK cluster that supports sandboxed containers.md)
-   [Use kubectl to connect to a cluster](/intl.en-US/User Guide for Kubernetes Clusters/Cluster management/Manage and access clusters/Use kubectl to connect to a cluster.md)

Sandboxed-Container enables your applications to run in sandboxed and lightweight virtual machines. This provides better isolation and higher security than runC containers. You can mount NAS file systems as volumes to sandboxed containers. In the community solution, a NAS file system is first mounted to a local directory of the host. Then, the local directory is shared to a container through 9pfs. However, 9pfs significantly degrades the I/O performance of containers. To improve the I/O performance, Alibaba Cloud Container Service for Kubernetes \(ACK\) allows you to directly mount NAS file systems to sandboxed containers by using the CSI plug-in. This feature allows you to mount a NAS file system to a container after the container is started. Previously, you can mount a NAS file system to only the host and only before the container is started. This improves the I/O performance because 9pfs is no longer required for a container to access a NAS file system.

**Note:** Sandboxed-Container V2 does not support direct NAS mounting.

Starting from Sandboxed-Container V1.1.0, ACK automatically enables this feature.

![Solutions to mount disks and NAS file systems to containers](../images/p95361.png "Comparison between the community solution and ACK solution to mount cloud disks and NAS file systems")

## How the direct mount solution works

![How the direct mount solution works](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8665359951/p95364.png)

The following table describes how to use the CSI plug-in to mount a NAS file system to a sandboxed container.

|Step|Description|
|----|-----------|
|①|The kubelet requests the CSI plug-in to mount a NAS file system.|
|②|The CSI plug-in sends a request to QueryServer to query whether a mounted volume that corresponds to the NAS file system exists. QueryServer is a local database that stores information about mounted volumes.|
|③|If no such mounted volume is found, information about the NAS file system, such as the mount target and mount directory, is written to QueryServer.|
|④|When a pod is ready, the kubelet starts to create a container. The request to mount the NAS file system is forwarded to Kata-Runtime.|
|⑤|Kata-Runtime sends a request to QueryServer to query information about the NAS file system. This includes the mount target and mount directory.|
|⑥|Kata-Runtime sends a request to Kata-Agent.|
|⑦|Kata-Agent starts a container and mounts the NAS file system to the container.|

## Examples

The following example describes how to mount a NAS file system to a sandboxed container. In this example, an Apsara File Storage NAS instance is created and a YAML file template is used to create resource objects.

1.  Create an Apsara File Storage NAS instance. For more information, see [Create a file system]().

    **Note:** The NAS file system must be deployed in the same VPC network as the cluster.

    Obtain the mount target of the NAS file system. Log on to the NAS console, find the created Apsara File Storage NAS. The URL \(file-system-id.region.nas.aliyuncs.com\) in the **Mount Command** column is the mount target, as shown in the following figure.

    ![NAS](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8665359951/p95499.png)

    **Note:** file-system-id.region.nas.aliyuncs.com is the address of the mount target. You can obtain the address of the mount target in the NAS console. Log on to the [NAS console](https://nas.console.aliyun.com/), click the name of the NAS file system. In the left-side navigation pane, click **Mounting Usage**. You can find the address of the mount target in the Mount Command column.

2.  Use the following template to create resource objects:

    **Note:** Replace $\{nas-server-address\} in the template with the address of the mount target.

    ```
    cat <<EOF | kubectl create -f -
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: nas-pvc-csi
      namespace: default
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 5Gi
      selector:
        matchLabels:
          alicloud-pvname: nas-pv-csi
    ---
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      labels:
        alicloud-pvname: nas-pv-csi
      name: nas-pv-csi
    spec:
      accessModes:
        - ReadWriteMany
      capacity:
        storage: 5Gi
      csi:
        driver: nasplugin.csi.alibabacloud.com
        volumeAttributes:
          options: noresvport,nolock
          path: /csi
          server: ${nas-server-address}
          vers: "3"
        volumeHandle: nas-pv-csi
      persistentVolumeReclaimPolicy: Retain
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: deploy-nas-csi
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: busybox
      template:
        metadata:
          labels:
            app: busybox
        spec:
          runtimeClassName: runv
          containers:
            - name: busybox
              image: registry.cn-hangzhou.aliyuncs.com/acs/busybox:v1.29.2
              command: 
              - tail
              - -f
              - /dev/null
              volumeMounts:
                - name: nas-pvc
                  mountPath: "/data"
          restartPolicy: Always
          volumes:
            - name: nas-pvc
              persistentVolumeClaim:
                claimName: nas-pvc-csi
    EOF
    ```

3.  Run the following commands to obtain the IDs of the created pods and query the type of the file systems that are mounted to the pods:

    ```
    kubectl get pods
    ```

    ```
    kubectl exec -it ${podid} sh
    ```

    ```
    mount | grep /data | grep nfs
    ```

    If the output is not empty, it indicates that the NAS file system is directly mounted to the container.


## Comparison of I/O performance in different mount modes

-   Input/output operations per second \(IOPS\) of random reads

    |Mount mode|Result|
    |----------|------|
    |Host|read: IOPS=24.3k, BW=94.8MiB/s \(99.4MB/s\)\(5687MiB/60004msec\)|
    |CSI plug-in|read: IOPS=26.0k, BW=102MiB/s \(107MB/s\)\(6101MiB/60005msec\)|
    |9PFS|read: IOPS=780, BW=3120KiB/s \(3195kB/s\)\(183MiB/60001msec\)|

-   IOPS of random writes

    |Mount mode|Result|
    |----------|------|
    |Host|write: IOPS=14.3k, BW=55.9MiB/s \(58.6MB/s\)\(3354MiB/60006msec\)|
    |CSI plug-in|write: IOPS=13.8k, BW=53.8MiB/s \(56.4MB/s\)\(3226MiB/60008msec\)|
    |9PFS|write: IOPS=356, BW=1428KiB/s \(1462kB/s\)\(83.7MiB/60001msec\)|

-   Throughput of random reads

    |Mount mode|Result|
    |----------|------|
    |Host|read: IOPS=331, BW=331MiB/s \(347MB/s\)\(29.9GiB/92439msec\)|
    |CSI plug-in|read: IOPS=251, BW=252MiB/s \(264MB/s\)\(29.9GiB/121561msec\)|
    |9PFS|read: IOPS=130, BW=130MiB/s \(136MB/s\)\(7861MiB/60422msec\)|

-   Throughput of random writes

    |Mount mode|Result|
    |----------|------|
    |Host|write: IOPS=507, BW=507MiB/s \(532MB/s\)\(29.9GiB/60372msec\)|
    |CSI plug-in|write: IOPS=508, BW=508MiB/s \(533MB/s\)\(29.9GiB/60255msec\)|
    |9PFS|write: IOPS=1, BW=1500KiB/s \(1536kB/s\)\(88.0MiB/60061msec\)|


The preceding results show that the throughput and IOPS are almost the same between a NAS file system that is mounted through the host and a NAS file system that is mounted by using CSI. However, a NAS file system that is mounted by using CSI has significant advantages in the IOPS than a NAS file system that is mounted through 9pfs.

