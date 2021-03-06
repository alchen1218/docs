---
title: Kubernetes Integration
tags: [integrations list]
permalink: kubernetes.html
summary: Learn about the Wavefront Kubernetes Integration.
---
## Kubernetes Integration

Wavefront provides a comprehensive solution for monitoring Kubernetes. This integration uses the [Wavefront Collector for Kubernetes](https://github.com/wavefrontHQ/wavefront-kubernetes-collector) to collect detailed metrics from Kubernetes clusters.

### Collection
The collector makes it easy for you to monitor and manage your Kubernetes environment:

* Collects real-time metrics from all layers of a Kubernetes environment (clusters, nodes, pods, containers and the Kubernetes control plane).
* Supports plugins such as Prometheus, Telegraf and Systemd enabling you to collect metrics from various workloads.
* [Auto discovery](https://github.com/wavefrontHQ/wavefront-kubernetes-collector/blob/master/docs/discovery.md) of pods and services based on annotation and configuration.
* Daemonset mode for high scalability with leader election for monitoring cluster-level resources.
* Rich [filtering](https://github.com/wavefrontHQ/wavefront-kubernetes-collector/blob/master/docs/filtering.md) support.
* Auto reload of configuration changes.
* [Internal metrics](https://github.com/wavefrontHQ/wavefront-kubernetes-collector/blob/master/docs/metrics.md#collector-health-metrics) for tracking the collector health and source of your Kubernetes metrics.

### Dashboards

In addition to setting up the metrics flow, this integration also installs dashboards:

* Kubernetes Summary: Detailed health of your infrastructure and workloads.
* Kubernetes Clusters: Detailed health of your clusters and its nodes, namespaces, pods and containers.
* Kubernetes Nodes: Detailed health of your nodes.
* Kubernetes Pods: Detailed health of your pods broken down by node and namespace.
* Kubernetes Containers: Deatailed health of your containers broken down by namespace, node and pod.
* Kubernetes Namespaces: Details of your pods/containers broken down by namespace.
* Kubernetes Collector Metrics: Internal stats of the Wavefront Kubernetes Collector.

Here's a preview of the Kubernetes Summary dashboard:

{% include image.md src="images/db_kubernetes_summary.png" width="80" %}

Here's a preview of the Kubernetes Pods dashboard:

{% include image.md src="images/db_kubernetes_pods.png" width="80" %}


## Kubernetes Quick Install Using Helm
1. Ensure that you have installed [helm](https://helm.sh/docs/intro/).
2. Add the Wavefront helm repo:{% raw %}
```
helm repo add wavefront https://wavefronthq.github.io/helm/
helm repo update
```
{% endraw %}
3. To deploy the Wavefront Collector and Wavefront Proxy:

    Using helm 2:{% raw %}
    ```
    helm install wavefront/wavefront --name wavefront --set wavefront.url=https://YOUR_CLUSTER.wavefront.com --set wavefront.token=YOUR_API_TOKEN --set clusterName=<YOUR_CLUSTER_NAME> --namespace wavefront
    ```
{% endraw %}
    Using helm 3:{% raw %}
    ```
    kubectl create namespace wavefront
    helm install wavefront wavefront/wavefront --set wavefront.url=https://YOUR_CLUSTER.wavefront.com --set wavefront.token=YOUR_API_TOKEN --set clusterName=<YOUR_CLUSTER_NAME> --namespace wavefront
    ```
{% endraw %}

**Note:** The `clusterName` property refers to the Kubernetes cluster, for example, `dev-cluster`. You must set this property. For Project Pacific, add `--set projectPacific.enabled=true` along with helm install command.

Refer to the Wavefront [helm chart](https://github.com/wavefrontHQ/helm/tree/master/wavefront) for further options.
## Kubernetes Manual Install
Follow the instructions below to manually set up Kubernetes monitoring.

### Step 1. Deploy a Wavefront Proxy in Kubernetes

1. Download [wavefront.yaml](https://raw.githubusercontent.com/wavefrontHQ/wavefront-kubernetes/master/wavefront-proxy/wavefront.yaml) to your system.
2. Edit the file and set `WAVEFRONT_URL` to `https://YOUR_CLUSTER.wavefront.com/api/` and `WAVEFRONT_TOKEN` to `YOUR_API_TOKEN`.
3. Run `kubectl create -f </path/to>/wavefront.yaml` to deploy the proxy.

The Wavefront proxy and a `wavefront-proxy` service should now be running in Kubernetes.

### Step 2. Deploy Wavefront Collector for Kubernetes

1. Create a directory named `wavefront-collector-dir` and download the following files to that directory:
  * [0-collector-namespace.yaml](https://raw.githubusercontent.com/wavefrontHQ/wavefront-kubernetes-collector/master/deploy/kubernetes/0-collector-namespace.yaml)
  * [1-collector-cluster-role.yaml](https://raw.githubusercontent.com/wavefrontHQ/wavefront-kubernetes-collector/master/deploy/kubernetes/1-collector-cluster-role.yaml)
  * [2-collector-rbac.yaml](https://raw.githubusercontent.com/wavefrontHQ/wavefront-kubernetes-collector/master/deploy/kubernetes/2-collector-rbac.yaml)
  * [3-collector-service-account.yaml](https://raw.githubusercontent.com/wavefrontHQ/wavefront-kubernetes-collector/master/deploy/kubernetes/3-collector-service-account.yaml)
  * [4-collector-config.yaml](https://raw.githubusercontent.com/wavefrontHQ/wavefront-collector-for-kubernetes/master/deploy/kubernetes/4-collector-config.yaml)
  * [5-collector-daemonset.yaml](https://raw.githubusercontent.com/wavefrontHQ/wavefront-collector-for-kubernetes/master/deploy/kubernetes/5-collector-daemonset.yaml)

2. Edit `4-collector-config.yaml` and replace `clusterName: k8s-cluster` with the name of your Kubernetes cluster.

3. If RBAC is disabled in your Kubernetes cluster, edit `5-collector-daemonset.yaml` and comment out `serviceAccountName: wavefront-collector`.

4. Run `kubectl create -f </path/to/wavefront-collector-dir>/` to deploy the collector on your cluster.

To verify the collector is deployed, run `kubectl get pods -n wavefront-collector`.

### Step 3. (Optional) Deploy the kube-state-metrics Service

The Wavefront Collector natively collects various [metrics](https://github.com/wavefrontHQ/wavefront-collector-for-kubernetes/blob/master/docs/metrics.md#kubernetes-state-source) about the state of Kubernetes resources. You can optionally deploy the third party [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) service to collect additional metrics.

To deploy kube-state-metrics:

1. Download [kube-state.yaml](https://raw.githubusercontent.com/wavefrontHQ/wavefront-kubernetes/master/ksm-all-in-one/kube-state.yaml) to your system.
2. Run `kubectl create -f </path/to>/kube-state.yaml`.

The `kube-state-metrics` service starts running on your cluster. The Wavefront Collector automatically discovers the service and starts collecting metrics from the kube-state-metrics service.
## Use OpenShift To Install Kubernetes

This page contains the Installation and Configuration steps for full-stack monitoring of OpenShift clusters using Wavefront Operator.

### Install and Configure Wavefront Operator on OpenShift Enterprise 3.x

The Wavefront Collector supports monitoring of OpenShift clusters:
  * To monitor OpenShift Origin 3.9 follow the steps in [Installation and Configuration on OpenShift](https://github.com/wavefronthq/wavefront-kubernetes-collector/tree/master/docs/openshift.md).
  * To monitor OpenShift Enterprise 3.11 follow the steps in [Installation and Configuration of Wavefront Collector Operator on OpenShift](https://github.com/wavefronthq/wavefront-kubernetes-collector/tree/master/docs/openshift-operator.md)

### Install and Configure Wavefront Operator on OpenShift Enterprise 4.x

This page contains the Installation and Configuration steps for full-stack monitoring of OpenShift clusters using Wavefront Operator.

#### Install and Configure Wavefront Operator 

1.  Log in to OpenShift Web UI as administrator.
2.  Create a project with name `wavefront`.
3.  In the Left pane, navigate to **Catalog** > **OperatorHub**.
4.  From the list of Operator types select **Monitoring** > **Wavefront**.
5.  Click on the **Wavefront Operator** and click **Install**.
6.  Select `wavefront` as namespace to subscribe to.
7.  When the subscription is successful, the operator is listed under **Installed Operators** and deploys Wavefront Proxy and Wavefront Collector CRD's into the project.
8.  Select **Installed Operators** > **Wavefront Operator** > **Wavefront Proxy** > **Create New**  to deploy the proxy.
9.  Create Wavefront Proxy custom resource by specifying the following parameters in the proxy spec and leave rest of the values as defaults.
    * token → YOUR_API_TOKEN
    * url → https://YOUR_CLUSTER.wavefront.com
10. Click **Create**.  This deploys the proxy service named `example-wavefrontproxy` with port 2878 as metric port.  In addition, Operator creates Persistent Volume Claim (PVC) with the name `wavefront-proxy-storage` using default underlying Persistent Volume (PV).
11. Select **Installed Operators** > **Wavefront Operator** > **Wavefront Collector** > **Create New** to deploy the collector.
12. Click **Create** without changing any values in the proxy definition.

Because default parameters are used, the collector runs as a daemonset and uses `example-wavefrontproxy` as sink. The collector auto-discovers the pods and services that expose metrics and dynamically starts collecting metrics for the targets. It collects metrics from the kubernetes API server if configured.

Now log in to Wavefront and search for the `openshift-demo-cluster` in kubernetes integration dashboards.

#### Using an Existing Proxy

To configure Wavefront Collector to use a proxy that's already running in your environment, follow these steps:
1. Download the example configuration [file](https://raw.githubusercontent.com/wavefrontHQ/wavefront-collector-for-kubernetes/master/deploy/examples/openshift-collector-configmap.yaml).
2. Update `sinks.proxyAddress` with your existing proxy address. Please refer this [document](https://github.com/wavefrontHQ/wavefront-collector-for-kubernetes/blob/master/docs/configuration.md) for more configuration options.
3. Create a ConfigMap by selecting **Workloads** > **ConfigMap** > **Select Project** > **Create ConfigMap**, copy paste the updated configuration in to the text field and click on **Create**.
4. Select **Installed Operators** > **Wavefront Operator** > **Wavefront Collector** > **Create New** to deploy the collector.
5. Set `spec.useOpenshiftDefaultConfig` to `false` and `spec.configName` to the ConfigMap name created in step 3.
6. Click on **Create**.


#### Advanced Wavefront Proxy Configuration

You can configure the proxy to change how it processes your data, port numbers, metric prefix etc. Refer this [document](https://docs.wavefront.com/proxies_configuring.html) for more details on proxy configuration properties. Below are the steps to create ConfigMap with advanced configuration properties:

1. Create a ConfigMap under the project where the operator is deployed.

   Example:-{% raw %}
   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: advanced-config
     namespace: wavefront
   data:
     wavefront.conf: |
       prefix = dev
       customSourceTags = <YOUR_K8S_CLUSTER>
   ```
{% endraw %}
2. Select **Installed Operators** > **Wavefront Operator** > **Wavefront Proxy** > **Create New** to deploy the proxy.
3. Set `spec.advanced` to the name of the ConfigMap created in Step 1.
4. Set `spec.token` to Wavefront API token and `spec.url` to Wavefront URL.
5. Click on **Create**.


#### Configuring Wavefront Proxy Preprocessor Rules

Preprocessor rules allow you to manipulate incoming metrics before they reach the proxy, for example, you could remove confidential text strings or replace unacceptable characters. See [Proxy Preprocessor Rules](https://docs.wavefront.com/proxies_preprocessor_rules.html) for details. Follow these steps to create a ConfigMap with custom preprocessor rules:

1. Create a ConfigMap under the project where the operator is deployed.
   
   Example:-{% raw %}
   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: preprocessor-config
     namespace: wavefront
   data:
      rules.yaml: |
        '2878':
          - rule    : add-cluster-tag
            action  : addTag
            tag     : env
            value   : dev
   ```
{% endraw %}
2. Select **Installed Operators** > **Wavefront Operator** > **Wavefront Proxy** > **Create New** to deploy the proxy.
3. Set `spec.preprocessor` to the name of the ConfigMap created in Step 1.
4. Set `spec.token` to Wavefront API token and `spec.url` to Wavefront URL.
5. Click on **Create**.

## Metrics

* [Kubernetes Source](#kubernetes-source)
* [Kubernetes State Source](#kubernetes-state-source)
* [Prometheus Source](#prometheus-source)
* [Systemd Source](#systemd-source)
* [Telegraf Source](#telegraf-source)
* [Collector Health](#collector-health-metrics)

This information comes directly from the [Wavefront Collector for Kubernetes github page](https://github.com/wavefrontHQ/wavefront-collector-for-kubernetes/blob/master/docs/metrics.md)

### Kubernetes Source

These metrics are collected from the `/stats/summary` endpoint on each [kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) running on a node.

Metrics collected per resource:

| Resource | Metrics |
|----------|---------|
| Cluster | CPU, Memory, Pod/Container counts |
| Namespace | CPU, Memory, Pod/Container counts |
| Nodes | CPU, Memory, Network, Filesystem, Storage, Uptime, Pod/Container counts |
| Pods | CPU, Memory, Network, Filesystem, Storage, Uptime, Restarts, Phase |
| Pod_Containers | CPU, Memory, Filesystem, Storage, Accelerator, Uptime, Restarts, Status |
| System_Containers | CPU, Memory, Uptime |

Metrics collected per type:

| Metric Name | Description |
|------------|-------------|
| cpu.limit | CPU hard limit in millicores. |
| cpu.node_capacity | CPU capacity of a node. |
| cpu.node_allocatable | CPU allocatable of a node in millicores. |
| cpu.node_reservation | Share of CPU that is reserved on the node allocatable in millicores. |
| cpu.node_utilization | CPU utilization as a share of node allocatable in millicores. |
| cpu.request | CPU request (the guaranteed amount of resources) in millicores. |
| cpu.usage | Cumulative amount of consumed CPU time on all cores in nanoseconds. |
| cpu.usage_rate | CPU usage on all cores in millicores. |
| cpu.load | CPU load in milliloads, i.e., runnable threads * 1000. |
| memory.limit | Memory hard limit in bytes. |
| memory.major_page_faults | Number of major page faults. |
| memory.major_page_faults_rate | Number of major page faults per second. |
| memory.node_capacity | Memory capacity of a node. |
| memory.node_allocatable | Memory allocatable of a node. |
| memory.node_reservation | Share of memory that is reserved on the node allocatable. |
| memory.node_utilization | Memory utilization as a share of memory allocatable. |
| memory.page_faults | Number of page faults. |
| memory.page_faults_rate | Number of page faults per second. |
| memory.request | Memory request (the guaranteed amount of resources) in bytes. |
| memory.usage | Total memory usage. |
| memory.cache | Cache memory usage. |
| memory.rss | RSS memory usage. |
| memory.working_set | Total working set usage. Working set is the memory being used and not easily dropped by the kernel. |
| network.rx | Cumulative number of bytes received over the network. |
| network.rx_errors | Cumulative number of errors while receiving over the network. |
| network.rx_errors_rate | Number of errors while receiving over the network per second. |
| network.rx_rate | Number of bytes received over the network per second. |
| network.tx | Cumulative number of bytes sent over the network. |
| network.tx_errors | Cumulative number of errors while sending over the network. |
| network.tx_errors_rate | Number of errors while sending over the network. |
| network.tx_rate | Number of bytes sent over the network per second. |
| filesystem.usage | Total number of bytes consumed on a filesystem. |
| filesystem.limit | The total size of filesystem in bytes. |
| filesystem.available | The number of available bytes remaining in a the filesystem. |
| filesystem.inodes | The number of available inodes in a the filesystem. |
| filesystem.inodes_free | The number of free inodes remaining in a the filesystem. |
| ephemeral_storage.limit | Local ephemeral storage hard limit in bytes. |
| ephemeral_storage.request | Local ephemeral storage request (the guaranteed amount of resources) in bytes. |
| ephemeral_storage.usage | Total local ephemeral storage usage. |
| ephemeral_storage.node_capacity | Local ephemeral storage capacity of a node. |
| ephemeral_storage.node_allocatable | Local ephemeral storage allocatable of a node. |
| ephemeral_storage.node_reservation | Share of local ephemeral storage that is reserved on the node allocatable. |
| ephemeral_storage.node_utilization | Local ephemeral utilization as a share of ephemeral storage allocatable. |
| accelerator.memory_total | Memory capacity of an accelerator. |
| accelerator.memory_used | Memory used of an accelerator. |
| accelerator.duty_cycle | Duty cycle of an accelerator. |
| accelerator.request | Number of accelerator devices requested by container. |
| uptime  | Number of milliseconds since the container was started. |
| <cluster, ns, node>.pod.count | Pod counts by cluster, namespaces and nodes. |
| <cluster, ns, node>.pod_container.count | Container counts by cluster, namespaces and nodes. |

### Kubernetes State Source

These are cluster level metrics about the state of Kubernetes objects collected by the Collector leader instance.

| Resource | Metric Name | Description |
|----------|---------|-------------|
| Deployment | deployment.desired_replicas | Number of desired pods. |
| Deployment | deployment.available_replicas | Total number of available pods (ready for at least minReadySeconds). |
| Deployment | deployment.ready_replicas | Total number of ready pods. |
| Replicaset | replicaset.desired_replicas | Number of desired replicas. |
| Replicaset | replicaset.available_replicas | Number of available replicas (ready for at least minReadySeconds). |
| Replicaset | replicaset.ready_replicas | Number of ready replicas. |
| Daemonset | daemonset.desired_scheduled | Total number of nodes that should be running the daemon pod. |
| Daemonset | daemonset.current_scheduled | Number of nodes that are running at least 1 daemon pod and are supposed to run the daemon pod. |
| Daemonset | daemonset.misscheduled | Number of nodes that are running the daemon pod, but are not supposed to run the daemon pod. |
| Daemonset | daemonset.ready | Number of nodes that should be running the daemon pod and have one or more of the daemon pod running and ready. |
| Statefulset | statefulset.desired_replicas | Number of desired replicas. |
| Statefulset | statefulset.current_replicas | Number of Pods created by the StatefulSet controller from the StatefulSet version indicated by currentRevision.
| Statefulset | statefulset.ready_replicas | Number of Pods created by the StatefulSet controller that have a Ready Condition. |
| Statefulset | statefulset.updated_replicas | Number of Pods created by the StatefulSet controller from the StatefulSet version indicated by updateRevision. |
| Job | job.active | Number of actively running pods. |
| Job | job.failed | Number of pods which reached phase Failed. |
| Job | job.succeeded | Number of pods which reached phase Succeeded. |
| Job | job.completions | Desired number of successfully finished pods the job should be run with. -1.0 indicates the value was not set. |
| Job | job.parallelism | Maximum desired number of pods the job should run at any given time. -1.0 indicates the value was not set. |
| CronJob | cronjob.active | Number of currently running jobs. |
| HorizontalPodAutoscaler | hpa.desired_replicas | Desired number of replicas of pods managed by this autoscaler as last calculated by the autoscaler. |
| HorizontalPodAutoscaler | hpa.min_replicas | Lower limit for the number of replicas to which the autoscaler can scale down. |
| HorizontalPodAutoscaler | hpa.max_replicas | Upper limit for the number of replicas to which the autoscaler can scale up. |
| HorizontalPodAutoscaler | hpa.current_replicas | Current number of replicas of pods managed by this autoscaler, as last seen by the autoscaler. |

### Prometheus Source

Varies by scrape target.

### Systemd Source

These are Linux systemd metrics that can be collected by each Collector instance.

| Metric Name | Description |
|------------|-------------|
| kubernetes.systemd.unit.state | Unit state (active, inactive etc). |
| kubernetes.systemd.unit.start.time.seconds | Start time of the unit since epoch in seconds. |
| kubernetes.systemd.system.running | Whether the system is operational (`systemctl is-system-running`). |
| kubernetes.systemd.units | Top level summary of systemd unit states (# of active, inactive units etc). |
| kubernetes.systemd.service.restart.total | Service unit count of Restart triggers. |
| kubernetes.systemd.timer.last.trigger.seconds | Seconds since epoch of last trigger. |
| kubernetes.systemd.socket.accepted.connections.total | Total number of accepted socket connections. |
| kubernetes.systemd.socket.current.connections | Current number of socket connections. |
| kubernetes.systemd_socket_refused_connections_total | Total number of refused socket connections. |

### Telegraf Source

Host metrics:

| Metric Prefix | Metrics Collected |
|------------|-------------|
| mem. | [metrics list](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/mem#metrics) |
| net. | [metrics list](https://github.com/influxdata/telegraf/blob/master/plugins/inputs/net/NET_README.md#measurements--fields) |
| netstat. | [metrics list](https://github.com/influxdata/telegraf/blob/master/plugins/inputs/net/NETSTAT_README.md#measurements) |
| linux.sysctl.fs. | [metrics list](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/linux_sysctl_fs#linux-sysctl-fs-input) |
| swap. | [metrics list](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/swap#metrics) |
| cpu. | [metrics list](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/cpu#measurements) |
| disk. | [metrics list](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/disk#metrics) |
| diskio. | [metrics list](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/diskio#metrics) |
| system. | [metrics list](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/system#metrics) |
| kernel. | [metrics list](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/kernel#measurements--fields) |
| processes. | [metrics list](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/processes#measurements--fields) |

Application metrics:

| Plugin Name | Metrics Collected |
|------------|-------------|
| activemq | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/activemq#measurements--fields) |
| apache | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/apache#measurements--fields) |
| consul | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/consul#metrics) |
| couchbase | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/couchbase#measurements) |
| couchdb | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/couchdb#measurements--fields) |
| haproxy | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/haproxy#metrics) |
| jolokia2 | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/jolokia2#jolokia2-input-plugins) |
| memcached | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/memcached#measurements--fields) |
| mongodb | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/mongodb#metrics) |
| mysql | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/mysql#metrics) |
| nginx | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/nginx#measurements--fields) |
| nginx plus | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/nginx_plus#measurements--fields) |
| postgresql | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/postgresql#postgresql-plugin) |
| rabbitmq | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/rabbitmq#measurements--fields) |
| redis | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/redis#measurements--fields) |
| riak | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/riak#measurements--fields) |
| zookeeper | [metrics list](https://github.com/influxdata/telegraf/tree/1.10.4/plugins/inputs/zookeeper#metrics) |

### Collector Health Metrics

These are internal metrics about the health and configuration of the Wavefront Collector.

| Metric Name | Description |
|------------|-------------|
| kubernetes.collector.discovery.enabled | Whether discovery is enabled. 0 (false) or 1 (true). |
| kubernetes.collector.discovery.rules.count | # of discovery configuration rules. |
| kubernetes.collector.discovery.targets.registered | # of auto discovered scrape targets currently being monitored. |
| kubernetes.collector.events.* | Events received, sent and filtered. |
| kubernetes.collector.leaderelection.error | leader election error counter. Only emitted in daemonset mode. |
| kubernetes.collector.leaderelection.leading | 1 indicates a pod is the leader. 0 (no). Only emitted in daemonset mode. |
| kubernetes.collector.runtime.* | Go runtime metrics (MemStats, NumGoroutine etc). |
| kubernetes.collector.sink.manager.timeouts | Counter of timeouts in sending data to Wavefront. |
| kubernetes.collector.source.manager.providers | # of configured source providers. Includes sources configured via auto-discovery. |
| kubernetes.collector.source.manager.scrape.errors | Scrape error counter across all sources. |
| kubernetes.collector.source.manager.scrape.latency.* | Scrape latencies across all sources. |
| kubernetes.collector.source.manager.scrape.timeouts | Scrape timeout counter across all sources. |
| kubernetes.collector.source.manager.sources | # of configured scrape targets. For example, a single Kubernetes source provider on a 10 node cluster will yield a count of 10. |
| kubernetes.collector.source.points.collected | collected points counter per source type. |
| kubernetes.collector.source.points.filtered | filtered points counter per source type. |
| kubernetes.collector.version | The version of the collector. |
| kubernetes.collector.wavefront.points.* | Wavefront sink points sent, filtered, errors etc. |
| kubernetes.collector.wavefront.events.* | Wavefront sink events sent, filtered, errors etc. |
| kubernetes.collector.wavefront.sender.type | 1 for proxy and 0 for direct ingestion. |




<!---
## Metrics


|Metric Name|Description|
| :--- | :--- |
|kubernetes.cluster.cpu.limit| |
|kubernetes.cluster.cpu.request| |
|kubernetes.cluster.cpu.usage_rate| |
|kubernetes.cluster.memory.limit| |
|kubernetes.cluster.memory.request| |
|kubernetes.cluster.memory.usage| |
|kubernetes.cluster.pod.count| |
|kubernetes.cluster.pod_container.count| |
|kubernetes.collector.discovery.enabled| |
|kubernetes.collector.discovery.rules.count| |
|kubernetes.collector.discovery.targets.registered| |
|kubernetes.collector.events.filtered| |
|kubernetes.collector.events.received| |
|kubernetes.collector.events.sent| |
|kubernetes.collector.leaderelection.error| |
|kubernetes.collector.leaderelection.leading| |
|kubernetes.collector.runtime.MemStats.Alloc| |
|kubernetes.collector.runtime.MemStats.BuckHashSys| |
|kubernetes.collector.runtime.MemStats.DebugGC| |
|kubernetes.collector.runtime.MemStats.EnableGC| |
|kubernetes.collector.runtime.MemStats.Frees| |
|kubernetes.collector.runtime.MemStats.GCCPUFraction| |
|kubernetes.collector.runtime.MemStats.HeapAlloc| |
|kubernetes.collector.runtime.MemStats.HeapIdle| |
|kubernetes.collector.runtime.MemStats.HeapInuse| |
|kubernetes.collector.runtime.MemStats.HeapObjects| |
|kubernetes.collector.runtime.MemStats.HeapReleased| |
|kubernetes.collector.runtime.MemStats.HeapSys| |
|kubernetes.collector.runtime.MemStats.LastGC| |
|kubernetes.collector.runtime.MemStats.Lookups| |
|kubernetes.collector.runtime.MemStats.MCacheInuse| |
|kubernetes.collector.runtime.MemStats.MCacheSys| |
|kubernetes.collector.runtime.MemStats.MSpanInuse| |
|kubernetes.collector.runtime.MemStats.MSpanSys| |
|kubernetes.collector.runtime.MemStats.Mallocs| |
|kubernetes.collector.runtime.MemStats.NextGC| |
|kubernetes.collector.runtime.MemStats.NumGC| |
|kubernetes.collector.runtime.MemStats.PauseNs.duration.p95| |
|kubernetes.collector.runtime.MemStats.PauseTotalNs| |
|kubernetes.collector.runtime.MemStats.StackInuse| |
|kubernetes.collector.runtime.MemStats.StackSys| |
|kubernetes.collector.runtime.MemStats.Sys| |
|kubernetes.collector.runtime.MemStats.TotalAlloc| |
|kubernetes.collector.runtime.NumCgoCall| |
|kubernetes.collector.runtime.NumGoroutine| |
|kubernetes.collector.runtime.NumThread| |
|kubernetes.collector.runtime.ReadMemStats.duration.p95| |
|kubernetes.collector.runtime.ReadMemStats.rate.count| |
|kubernetes.collector.runtime.ReadMemStats.rate.m1| |
|kubernetes.collector.sink.manager.timeouts| |
|kubernetes.collector.source.collect.errors| |
|kubernetes.collector.source.manager.providers| |
|kubernetes.collector.source.manager.scrape.errors| |
|kubernetes.collector.source.manager.scrape.latency.duration.p95| |
|kubernetes.collector.source.manager.scrape.timeouts| |
|kubernetes.collector.source.points.collected| |
|kubernetes.collector.source.points.filtered| |
|kubernetes.collector.target.collect.errors| |
|kubernetes.collector.target.points.collected| |
|kubernetes.collector.version| |
|kubernetes.collector.wavefront.events.sent.count| |
|kubernetes.collector.wavefront.points.errors.count| |
|kubernetes.collector.wavefront.points.filtered.count| |
|kubernetes.collector.wavefront.points.metric-sets.count| |
|kubernetes.collector.wavefront.points.sent.count| |
|kubernetes.collector.wavefront.sender.type| |
|kubernetes.daemonset.current_scheduled| |
|kubernetes.daemonset.desired_scheduled| |
|kubernetes.daemonset.misscheduled| |
|kubernetes.daemonset.ready| |
|kubernetes.deployment.available_replicas| |
|kubernetes.deployment.desired_replicas| |
|kubernetes.deployment.ready_replicas| |
|kubernetes.node.cpu.limit| |
|kubernetes.node.cpu.node_allocatable| |
|kubernetes.node.cpu.node_capacity| |
|kubernetes.node.cpu.node_reservation| |
|kubernetes.node.cpu.node_utilization| |
|kubernetes.node.cpu.request| |
|kubernetes.node.cpu.usage| |
|kubernetes.node.cpu.usage_rate| |
|kubernetes.node.ephemeral_storage.limit| |
|kubernetes.node.ephemeral_storage.node_allocatable| |
|kubernetes.node.ephemeral_storage.node_capacity| |
|kubernetes.node.ephemeral_storage.node_reservation| |
|kubernetes.node.ephemeral_storage.node_utilization| |
|kubernetes.node.ephemeral_storage.request| |
|kubernetes.node.ephemeral_storage.usage| |
|kubernetes.node.filesystem.available| |
|kubernetes.node.filesystem.inodes| |
|kubernetes.node.filesystem.inodes_free| |
|kubernetes.node.filesystem.limit| |
|kubernetes.node.filesystem.usage| |
|kubernetes.node.memory.limit| |
|kubernetes.node.memory.major_page_faults| |
|kubernetes.node.memory.major_page_faults_rate| |
|kubernetes.node.memory.node_allocatable| |
|kubernetes.node.memory.node_capacity| |
|kubernetes.node.memory.node_reservation| |
|kubernetes.node.memory.node_utilization| |
|kubernetes.node.memory.page_faults| |
|kubernetes.node.memory.page_faults_rate| |
|kubernetes.node.memory.request| |
|kubernetes.node.memory.rss| |
|kubernetes.node.memory.usage| |
|kubernetes.node.memory.working_set| |
|kubernetes.node.network.rx| |
|kubernetes.node.network.rx_errors| |
|kubernetes.node.network.rx_errors_rate| |
|kubernetes.node.network.rx_rate| |
|kubernetes.node.network.tx| |
|kubernetes.node.network.tx_errors| |
|kubernetes.node.network.tx_errors_rate| |
|kubernetes.node.network.tx_rate| |
|kubernetes.node.pod.count| |
|kubernetes.node.pod_container.count| |
|kubernetes.node.status.condition| |
|kubernetes.node.uptime| |
|kubernetes.ns.cpu.limit| |
|kubernetes.ns.cpu.request| |
|kubernetes.ns.cpu.usage_rate| |
|kubernetes.ns.memory.limit| |
|kubernetes.ns.memory.request| |
|kubernetes.ns.memory.usage| |
|kubernetes.ns.pod.count| |
|kubernetes.ns.pod_container.count| |
|kubernetes.pod.cpu.limit| |
|kubernetes.pod.cpu.request| |
|kubernetes.pod.cpu.usage| |
|kubernetes.pod.cpu.usage_rate| |
|kubernetes.pod.ephemeral_storage.limit| |
|kubernetes.pod.ephemeral_storage.request| |
|kubernetes.pod.ephemeral_storage.usage| |
|kubernetes.pod.filesystem.available| |
|kubernetes.pod.filesystem.inodes| |
|kubernetes.pod.filesystem.inodes_free| |
|kubernetes.pod.filesystem.limit| |
|kubernetes.pod.filesystem.usage| |
|kubernetes.pod.memory.limit| |
|kubernetes.pod.memory.major_page_faults| |
|kubernetes.pod.memory.major_page_faults_rate| |
|kubernetes.pod.memory.page_faults| |
|kubernetes.pod.memory.page_faults_rate| |
|kubernetes.pod.memory.request| |
|kubernetes.pod.memory.rss| |
|kubernetes.pod.memory.usage| |
|kubernetes.pod.memory.working_set| |
|kubernetes.pod.network.rx| |
|kubernetes.pod.network.rx_errors| |
|kubernetes.pod.network.rx_errors_rate| |
|kubernetes.pod.network.rx_rate| |
|kubernetes.pod.network.tx| |
|kubernetes.pod.network.tx_errors| |
|kubernetes.pod.network.tx_errors_rate| |
|kubernetes.pod.network.tx_rate| |
|kubernetes.pod.restart_count| |
|kubernetes.pod.status.phase| |
|kubernetes.pod.uptime| |
|kubernetes.pod_container.cpu.limit| |
|kubernetes.pod_container.cpu.request| |
|kubernetes.pod_container.cpu.usage| |
|kubernetes.pod_container.cpu.usage_rate| |
|kubernetes.pod_container.ephemeral_storage.limit| |
|kubernetes.pod_container.ephemeral_storage.request| |
|kubernetes.pod_container.ephemeral_storage.usage| |
|kubernetes.pod_container.filesystem.available| |
|kubernetes.pod_container.filesystem.inodes| |
|kubernetes.pod_container.filesystem.inodes_free| |
|kubernetes.pod_container.filesystem.limit| |
|kubernetes.pod_container.filesystem.usage| |
|kubernetes.pod_container.memory.limit| |
|kubernetes.pod_container.memory.major_page_faults| |
|kubernetes.pod_container.memory.major_page_faults_rate| |
|kubernetes.pod_container.memory.page_faults| |
|kubernetes.pod_container.memory.page_faults_rate| |
|kubernetes.pod_container.memory.request| |
|kubernetes.pod_container.memory.rss| |
|kubernetes.pod_container.memory.usage| |
|kubernetes.pod_container.memory.working_set| |
|kubernetes.pod_container.restart_count| |
|kubernetes.pod_container.status| |
|kubernetes.pod_container.uptime| |
|kubernetes.replicaset.available_replicas| |
|kubernetes.replicaset.desired_replicas| |
|kubernetes.replicaset.ready_replicas| |
|kubernetes.statefulset.current_replicas| |
|kubernetes.statefulset.desired_replicas| |
|kubernetes.statefulset.ready_replicas| |
|kubernetes.statefulset.updated_replicas| |
|kubernetes.sys_container.cpu.usage| |
|kubernetes.sys_container.cpu.usage_rate| |
|kubernetes.sys_container.memory.major_page_faults| |
|kubernetes.sys_container.memory.major_page_faults_rate| |
|kubernetes.sys_container.memory.page_faults| |
|kubernetes.sys_container.memory.page_faults_rate| |
|kubernetes.sys_container.memory.rss| |
|kubernetes.sys_container.memory.usage| |
|kubernetes.sys_container.memory.working_set| |
|kubernetes.sys_container.uptime| |
|kubernetes.systemd.socket.accepted.connections.total| |
|kubernetes.systemd.socket.current.connections| |
|kubernetes.systemd.system.running| |
|kubernetes.systemd.timer.last.trigger.seconds| |
|kubernetes.systemd.unit.state| |
|kubernetes.systemd.units| |
|   |   |
--->

<h2>Alerts</h2>  <ul><li markdown="span"><b>K8s pod CPU usage too high</b>:Alert reports when the CPU millicore utilization of a pod exceeds the CPU millicore limit defined constantly. Having the CPU going over the set limit will cause the pod to suffer from CPU throttling which is going to affect the pod's performance. When this happens, please make sure the CPU resource limitation set for the pod is correctly configured.</li><li markdown="span"><b>K8s pod memory usage too high</b>:Alert reports when a pod's memory utilization is constantly at high percentage.</li><li markdown="span"><b>K8s too many pods crashing</b>:Alert reports when a pod's running and succeeded phase percentage is below the required level specified.</li><li markdown="span"><b>K8s node CPU usage too high</b>:Alert reports when a node's cpu utilization percentage is constantly high.</li><li markdown="span"><b>K8s node storage usage too high</b>:Alert reports when a node's storage is almost full.</li><li markdown="span"><b>K8s node memory usage too high</b>:Alert reports when node's memory utilization is constantly high.</li><li markdown="span"><b>K8s too many containers not running</b>:Alert reports when the percentage of containers not running is constantly high.</li><li markdown="span"><b>K8s node unhealthy</b>:Alert reports when a node's condition is not ready or status is not true.</li><li markdown="span"><b>K8s pod storage usage too high</b>:Alerts reports when the pod's storage is almost full.</li></ul>