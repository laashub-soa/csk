# Kubernetes 1.12 release notes

The Kubernetes version used by Container Service for Kubernetes \(ACK\) is fully compatible with the Kubernetes community and shares the core code with the community. This topic describes the changes made to Kubernetes 1.12 by ACK.

## 1.12.6-aliyun.1

-   Shields the logs of TLS handshake errors between kube-apiserver and kubelet that are generated during health checks for Server Load Balancer \(SLB\) instances.

    CommitID: 4f1d96e153b050d8374bfbb66803d7b3d9181abe

-   kubeadm can perform version verification on Docker 18.09.2.

    CommitID: 3b1ebfa1b857c44f5261a36f1420b10a08e01771

-   Changes the level of logs that can be watched for aggregationcontroller.

    CommitID: 01a904eed3f9486caa482c8983698075d1cea2f1

-   Kubeadm
    -   kubeadm automatically performs retries for interactions between kubeadm and apiserver when cluster resources are updated.
    -   kubeadm no longer deploys the Domain Name Server \(DNS\) server.
    -   kubeadm no longer deploys kube-proxy.
    -   The validity period of a certificate generated by kubeadm is 10 years.

For more information, see [Kubernetes 1.12.6 release notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.12.md#v1126).

