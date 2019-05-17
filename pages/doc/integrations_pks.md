---
title: VMware Enterprise PKS Integration Details
keywords:
tags: [integrations, dashboards]
sidebar: doc_sidebar
permalink: integrations_pks.html
summary: Learn about predefined alerts, code examples, and more
---
The Wavefront [VMware Enterprise PKS integration](pks.html) includes an overview and setup instructions.

On this page, we list predefined alerts and give other details about the integration.

## Predefined Alerts for the Integration

The VMware Enterprise PKS integration provides the following monitoring alerts for PKS. These alerts are predefined, no additional setup is required. You can [create additional alerts](alerts.html#creating-an-alert) from the **Alerts** browser or from charts in the Wavefront UI.

<table>
<tbody>
<thead>
<tr><th width="60%">Name</th><th width="20%">Severity</th><th width="20%">Resolve After (min)</th></tr>
</thead>
<tr>
<td>Node Memory Usage high</td>
<td>WARN</td>
<td>10</td></tr>
<tr>
<td>Node Memory Usage too high</td>
<td>SEVERE</td>
<td>10</td></tr>
<tr>
<td>Node CPU Usage high</td>
<td>WARN</td>
<td>5</td></tr>
<tr>
<td>Node CPU Usage too high</td>
<td>SEVERE</td>
<td>5</td></tr>
<tr>
<td>Node Storage Usage high</td>
<td>WARN</td>
<td>10</td></tr>
<tr>
<td>Node Storage Usage too high</td>
<td>SEVERE</td>
<td>10</td></tr>
<tr>
<td>Too many Pods crashing</td>
<td>SEVERE</td>
<td>5</td></tr>
<tr>
<td>Too many Containers not running</td>
<td>SEVERE</td>
<td>5</td></tr>
<tr>
<td>Node unhealthy</td>
<td>SEVERE</td>
<td>5</td></tr>
</tbody>
</table>


## VMware EnterprisePKS Monitoring Dashboards

Wavefront includes several predefined dashboards for monitoring PKS. You can use these dashboards as is, or [clone and customize them](ui_dashboards.html).

We support dashboards for major functionality including:
* PKS Home dashboard
* Nodes dashboard
* Namespaces dashboard
* Deployments dashboard
* Pods dashbord
* Pod containers dashboard
* Services and Replication Sets dashboard

To see the full set of dashboards, sign in your Wavefront instance -- or sign up for a [free trial](http://wavefront.com/sign-up/?utm_source=docs.vmware.com&utm_medium=referral&utm_campaign=docs-front-page){:target="_blank" rel="noopenner noreferrer"}.


## Architecture

The Wavefront proxy pod includes four containers.
* The [Wavefront Kubernetes Collector](https://github.com/wavefrontHQ/wavefront-kubernetes-collector)  monitors your worker kubelets and sends the result to the proxy.
* Telegraf receives metrics about the node, pod, and container status from kube-state-metrics, and sends those metrics to the Wavefront proxy as well.

![pks-arch](images/pks-architecture-rev.png)

Wavefront runs a Wavefront proxy pod inside each PKS-created Kubernetes cluster. There are four containers within the Wavefront proxy pod.


## Troubleshooting the VMware PKS Integration

### 401 Unauthorized Error

If PKS tile deployment fails at running the `wavefront-alert-creation` errand with `401 Unauthorized`, the `wavefront-access-token` is invalid.

### No such host/no route to host Error
If you receive a no `such host/no route to host` error, check that the wavefront-api-server can connect to the Internet, as follows:

```
curl -s 'https://vmware.wavefront.com/api/v2/source‘ \
-H 'Authorization: Bearer 1d23d456-XXXX-XXXX-XXXX-f123f12b1c21'| jq .
```
### No Metrics

If you see  no metrics in the Wavefront dashboard, possible causes are:
- 401 unauthorized: check wavefront-access-token validity
- Network connectivity to wavefront-api-server

Check the status of the wavefront-proxy pod:
```
kubectl get pods --all-namespaces
kubectl describe pod wavefront-proxy-pod-name -n kube-system
```

Check wavefront-proxy pod logs:
```
kubectl logs wavefront-proxy-pod-name -n kube-system -c wavefront-proxy
kubectl logs wavefront-proxy-pod-name -n kube-system -c wavefront-collector
kubectl logs wavefront-proxy-pod-name -n kube-system -c kube-state-metrics
```
