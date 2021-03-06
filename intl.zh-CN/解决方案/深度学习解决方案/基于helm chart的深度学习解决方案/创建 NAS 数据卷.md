# 创建 NAS 数据卷 {#task_bh5_dll_vdb .task}

本文为您介绍如何创建NAS数据卷。

准备一个与 Kubernetes 集群位于相同地域的存储包。您可使用已有的存储包，或者购买一个新的存储包，请参见[购买存储包](../../../../intl.zh-CN/产品定价/包年包月/购买存储包.md#)。

1.  登录 [NAS控制台](https://nas.console.aliyun.com/)。
2.  默认进入文件系统列表页面，选择与 GPU 集群相同的地域，单击右上角 **创建文件系统**。 

    ![创建系统文件](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16631/15682557169093_zh-CN.png)

3.  在弹出的对话框中，进行配置，创建一个 NAS 文件系统。 

    ![配置系统文件](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16631/15682557169094_zh-CN.png)

    -   **地域**：选择与容器集群相同的地域。本示例选择 **华东1**。
    -   **存储类型**：本示例选择 容量型 。
    -   **协议类型**：选择 NFS。
    -   **可用区**：选择 **华东1可用区B**。同一地域不同可用区可以互通。
    -   **存储包**：选择一个准备好的存储包。
4.  单击 **确定** 后，新建的文件系统出现在列表中。
5.  单击新建文件系统右侧的 **管理**，进入文件系统详情页面。 

    ![文件详情页面](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16631/15682557169095_zh-CN.png)

6.  单击 **添加挂载点**，添加 VPC 类型的挂载点，参见 [步骤二：添加挂载点](../../../../intl.zh-CN/快速入门/容量型__性能型NAS/Linux系统.md#section_9q8_owp_z6n)。 

    ![添加挂载点](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16631/15682557169096_zh-CN.png)

7.  添加挂载点完毕后，查看挂载点地址。 

    ![查看挂载地址](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16631/15682557179097_zh-CN.png)


