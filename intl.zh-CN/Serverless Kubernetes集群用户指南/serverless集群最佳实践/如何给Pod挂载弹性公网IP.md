# 如何给Pod挂载弹性公网IP

本文主要为您介绍如何在Serverless Kubernetes或虚拟节点中给Pod挂载EIP。

阿里云Serverless Kubernetes服务、虚拟节点推出Pod挂载弹性公网IP功能，此功能使某些Serverless容器应用的部署和服务访问变得更加简单和便利。

-   无需创建VPC NAT网关即可让单个Pod访问公网。
-   无需创建Service也可让单个Pod暴露公网服务。
-   可以更加灵活而且动态的绑定Pod和EIP。

## 前提条件

-   创建一个[Serverless Kubernetes集群](/intl.zh-CN/Serverless Kubernetes集群用户指南/快速入门/创建Serverless Kubernetes集群.md)或在Kubernetes集群创建一个[部署虚拟节点Chart](/intl.zh-CN/Kubernetes集群用户指南/弹性容器实例ECI/部署虚拟节点Chart.md)。
-   请确保该集群的安全组已开放相关端口（本示例中需要开放80端口）。

## 操作步骤

您可以通过以下两种方法给Pod挂载弹性公网IP。

## 方法一：自动分配弹性公网EIP

1.  登录[容器服务管理控制台](https://cs.console.aliyun.com)。

2.  在控制台左侧导航栏中，单击**Serverless集群**。

3.  在集群列表页面中，单击目标集群名称或者目标集群右侧**操作**列下的**详情**。

4.  在集群管理页左侧导航栏中，单击**工作负载**。

5.  单击**无状态**页签，然后单击**使用模板创建**，选择示例模板或自定义，然后单击**创建**。

    您可以使用如下YAML示例模板创建Pod。本例中，通过指定k8s.aliyun.com/eci-with-eip为true，Serverless Kubernetes服务和虚拟节点会自动为此Pod分配一个EIP，并且绑定到Pod上。

    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx
      annotations:
        "**k8s.aliyun.com/eci-with-eip**": "**true**"
    spec:
      containers:
      - image: registry-vpc.cn-hangzhou.aliyuncs.com/jovi/nginx:alpine
        imagePullPolicy: Always
        name: nginx
        ports:
        - containerPort: 80
          name: http
          protocol: TCP
      restartPolicy: OnFailure
    ```

    **说明：**

    -   您可以通过Annotation k8s.aliyun.com/eip-bandwidth指定EIP的带宽，默认值为5，单位为M。
    -   您也可以通过Annotation k8s.aliyun.com/eip-common-bandwith-package-id让EIP绑定共享带宽。
    -   如果您创建的是Deployment，那么Deployment中的每一个Pod都将会被挂载不同的EIP，请谨慎使用此操作。
6.  单击**容器组**页签，查看容器组的状态。

7.  在目标容器组右侧单击**编辑**，弹出编辑YAML文件。

    **说明：** YAML文件中`k8s.aliyun.com/allocated-eipAddress: 47.110.XX.XX`的IP地址即为EIP的公网访问地址。

8.  在浏览器中输入http://ip地址，您可访问nginx欢迎页。

    此处的http://ip地址为YAML文件中`k8s.aliyun.com/allocated-eipAddress: 47.110.XX.XX`的IP地址。

    ![nginx欢迎页](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3563659951/p51465.png)

    **说明：** 此方式中EIP为动态分配，其生命周期与Pod相同，当删除Pod时，动态分配的EIP也会一并删除。


## 方法二：指定弹性公网IP实例ID

1.  登录[VPC管理控制台](https://vpc.console.aliyun.com)，购买弹性公网IP。请参见[申请新EIP](/intl.zh-CN/用户指南/申请EIP/申请新EIP.md)。

    **说明：** 申请的EIP和集群必须在同一区域。

2.  登录[容器服务管理控制台](https://cs.console.aliyun.com)。

3.  在控制台左侧导航栏中，单击**Serverless集群**。

4.  在集群列表页面中，单击目标集群名称或者目标集群右侧**操作**列下的**详情**。

5.  在集群管理页左侧导航栏中，单击**工作负载**。

6.  单击**无状态**页签，然后单击**使用模板创建**，选择示例模板或自定义，然后单击**创建**。

    您可以使用如下YAML示例模板创建Pod。本例中，通过指定Pod的Annonation k8s.aliyun.com/eci-eip-instanceid为EIP实例ID。

    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx
      annotations:
        "**k8s.aliyun.com/eci-eip-instanceid**": "**<youreipInstanceId\>**"
    spec:
      containers:
      - image: registry-vpc.cn-hangzhou.aliyuncs.com/jovi/nginx:alpine
        imagePullPolicy: Always
        name: nginx
        ports:
        - containerPort: 80
          name: http
          protocol: TCP
      restartPolicy: OnFailure
    ```

    **说明：** `<youreipInstanceId>`需要替换成步骤1中获取的EIP实例ID。

7.  单击**容器组**页签，查看容器组的状态。

8.  在浏览器中输入http://ip地址，您可访问nginx欢迎页。

    ![nginx欢迎页](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3563659951/p47613.png)

    **说明：** 此处的http://ip地址为步骤1中申请的EIP的IP地址。


