# Use Knative to handle GitHub events

This topic describes how to handle GitHub events by using Knative.

-   Knative Serving, Knative Eventing, and the GitHub add-on are deployed in your Container Service for Kubernetes \(ACK\) cluster. For more information, see [Deploy components](/intl.en-US/User Guide for Kubernetes Clusters/Knative management/Manage Knative components/Deploy a Knative component.md).
-   A custom domain name is configured in Knative. For more information, see [Set up a custom domain name for Knative Serving](/intl.en-US/User Guide for Kubernetes Clusters/Knative management/Manage Knative services/Use a custom domain name in Knative.md).

## Create a Knative service

1.  Log on to the [ACK console](https://cs.console.aliyun.com).

2.  On the Clusters page, click the name of a cluster or click **Details** in the **Actions** column. The details page of the cluster appears.

3.  In the left-side navigation pane, choose **Knative** \> **Services**.

4.  On the Knative Services page, click **Create Service** in the upper-right corner of the page.

5.  On the Create Service page, select the cluster and namespace where you want to deploy the Knative service, enter a name for the service, and then select an image and image version.

    **Note:**

    In this example, the parameters are specified based on the following information:

    -   **Service Name**: Enter a name for the service. The name is github-event-display in this example.
    -   **Image Name**: Click Select Image. In the dialog box that appears, select an image and click **OK**. You can also enter the address of a private image repository. The address must be in the domainname/namespace/imagename:tag format. This parameter is set to registry.cn-shanghai.aliyuncs.com/knative-release/eventing-sources-event\_display in this example.
    -   **Image Version**: Click Select Image Version and select an image version. If you do not specify an image version, the latest version is used. The image version is bf45b3eb1e7fc4cb63d6a5a6416cf696295484a7662e0cf9ccdf5c080542c21d in this example.
    For more information about other parameters, see [Parameter descriptions](/intl.en-US/User Guide for Kubernetes Clusters/Knative management/Manage Knative services/Create a Knative Service.md).

6.  Click **Create**.


## Create a GitHub token

1.  Create a personal access token.

    A personal access token is required when you call the GitHub API. For more information, see [Personal access tokens](https://github.com/settings/tokens).

    The following figure shows how to create a token named GitHubSource Sample.

    ![Create a GitHub token](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0165359951/p58037.png)

    **Note:** To trigger events from your public repositories and create webhooks for those repositories, select `repo:public_repo` and `admin:repo_hook`.

    You can enter a custom token name.

2.  [Use kubectl to connect to an ACK cluster](/intl.en-US/User Guide for Kubernetes Clusters/Cluster management/Access clusters/Use kubectl to connect to an ACK cluster.md).

3.  Use the following method to generate a random string as secretToken:

    ```
    head -c 8 /dev/urandom | base64
    ```

4.  Update the githubsecret.yaml file.

    If the generated token is personal\_access\_token\_value, you must set the secretToken field. The following code block is an example:

    ```
    apiVersion: v1
    kind: Secret
    metadata:
      name: githubsecret
    type: Opaque
    stringData:
      accessToken: personal_access_token_value
      secretToken: asdfasfdsaf
    ```

5.  Run the following command to create a GitHub token:

    ```
    kubectl --namespace default apply githubsecret.yaml
    ```


## Create a GitHub event source

You can create a GitHub event source to receive events generated by GitHub.

1.  [Use kubectl to connect to an ACK cluster](/intl.en-US/User Guide for Kubernetes Clusters/Cluster management/Access clusters/Use kubectl to connect to an ACK cluster.md).

2.  Create the github-source.yaml file and copy the following content to the file:

    ```
    apiVersion: sources.eventing.knative.dev/v1alpha1
    kind: GitHubSource
    metadata:
      name: githubsourcesample
    spec:
      eventTypes:
        - pull_request
      ownerAndRepository: <YOUR USER>/<YOUR REPO>
      accessToken:
        secretKeyRef:
          name: githubsecret
          key: accessToken
      secretToken:
        secretKeyRef:
          name: githubsecret
          key: secretToken
      sink:
        apiVersion: serving.knative.dev/v1alpha1
        kind: Service
        name: github-event-display
    ```

3.  Run the following command to create a GitHub event source in the default namespace:

    ```
    kubectl --namespace default apply github-source.yaml
    ```


In your GitHub repository, choose **Settings** \> **Webhooks** and check whether a verified hook URL is displayed.

**Note:** The domain name has an Internet Content Provider \(ICP\) number. Otherwise, the domain name cannot be accessed.

Run the following command in your GitHub repository to create a `pull request`. Then, an event is triggered.

```
kubectl --namespace default get pods
kubectl --namespace default logs github-event-display-XXXX user-container
```

In Knative Eventing, you can view event details that are similar to the following content:

```
2018/11/08 18:25:34 Message Dumper received a message: POST / HTTP/1.1
Host: github-event-display.knative-demo.svc.cluster.local
Accept-Encoding: gzip
Ce-Cloudeventsversion: 0.1
Ce-Eventid: a8d4cf20-e383-11e8-8069-46e3c8ad****
Ce-Eventtime: 2018-11-08T18:25:32.819548012Z
Ce-Eventtype: dev.knative.source.github.pull_request
Ce-Source: https://github.com/someuser/somerepo/pull/1
Content-Length: 21060
Content-Type: application/json
User-Agent: Go-http-client/1.1
X-B3-Parentspanid: b2e514c3dbe94c03
X-B3-Sampled: 1
X-B3-Spanid: c85e346d89c8be4e
X-B3-Traceid: abf6292d458fb8e7
X-Envoy-Expected-Rq-Timeout-Ms: 60000
X-Envoy-Internal: true
X-Forwarded-For: 12*. *. *. *, 12*. *. *. *
X-Forwarded-Proto: http
X-Request-Id: 8a2201af-5075-9447-b593-ec3a243a****

{"action":"opened","number":1,"pull_request": ...}
```
