---
title: Install and Manage Wavefront Proxies
tags: [proxies, best practice]
sidebar: doc_sidebar
permalink: proxies_installing.html
summary: Learn how to install and manage Wavefront proxies.
---
In most cases, a Wavefront proxy must be running in your installation before metrics begin streaming to Wavefront from a host or application.

We offer several [deployment options](proxies.html#proxy-deployment-options). During development, a single proxy is often sufficient for all data sources. In production, place a team of proxies behind a load balancer.

## Installation Types

You can install a proxy from the UI or perform a package install.

{% include shared/permissions_view.html entity="proxies" entitymgmt="Proxy" %}

### Install from the UI

You can install a Wavefront proxy from the UI:

* As part of an integration. Many integrations send data to a Wavefront proxy. As part of the integration setup, you're prompted to select a proxy that already exists in your environment or add a proxy.
* Explicitly from the UI using **Browse > Proxies** as discussed below.

### Package Install

You can install a proxy explicitly on a host or container. [Installing a proxy manually](proxies_manual_install.html) gives steps for different use cases, including install on hosts with limited network connectivity.

If you install a proxy into a container, you might have to [customize your setup](proxies_configuring.html#configuring-a-proxy-in-a-container).
<!---Vasily to decide whether we still want to mention this here, container install now much easier--->

<!--Vasily: cut this?? We're saying it later...
If you don't use the Wavefront UI to install the proxy, the installation procedures might require:
* A Wavefront API URL in the format `https://<wavefront_instance>.wavefront.com/api/`
* An [API token](wavefront_api.html#generating-an-api-token), which you generate for your instance.
--->


## Proxy Host Requirements

<!---Vasily: This section was added by request from Mike E. Is it useful? (esp OS versions!)--->

- Internet access - run `timeout 3s curl -fIsS <wavefront_api_url>` from the host and make sure you get a response and not a timeout.
- Networking - For **metrics**, the proxy uses port 2878 by default. If you want to change this default, or if you want to set up ports for histograms or trace data, see [Set the Listener Port for Metrics, Histograms, and Traces](proxies_installing.html#set-the-listener-port-for-metrics-histograms-and-traces).
- Memory - The proxy does not use a lot of CPU, memory, or storage. However, we recommend running the proxy on a host with at least 4GB of free memory.
- Operating system
  - Linux: We've tested the proxy with the following versions.
    - Ubuntu 14.04, 16.04, 18.04
    - CentOS 6.5, 7
    - RHEL 6, 7
    - Debian 7, 8, 9, 10
    - Amazon Linux
  - Mac - MacOS Sierra (10.12) and later
  - Windows - Windows 8 and later

You can also run a proxy in a [Docker or Kubernetes container](proxies_configuring.html#configuring-a-proxy-in-a-container).

<a name="single"></a>

## Install a Proxy

Many users install a proxy when they set up an integration. For other situation, we support several installation options.

{% include note.html content="In development, many customers use only one proxy that receives data from many applications and sends those data to the Wavefront service. In production, consider using two proxies behind a load balancer. See [Proxy Deployment Options](proxies.html#proxy-deployment-options)." %}

### Install a Proxy from the UI

To install and run a proxy on a Linux, Mac, or Windows host, or in a Docker container on a host:

1. Open the Wavefront application UI.
1. Select **Browse > Proxies**.
1. Select **Add > New Proxy** at the top of the filter bar.
1. Click the **\[Linux \| Mac \| Windows \| Docker ]** tab.
1. (Windows Only) Download the proxy.
1. Copy the script and run it on your host.
    {% include note.html content="On Windows, do not run the installer `.exe` file. Run the script instead." %}
1. After the proxy contacts the Wavefront service, the proxy name displays under "Checking for new proxies..." and the button label changes to **Done**.
1. Click **Done** and verify that your proxy is listed on the Proxies page. If not, follow the steps in [Managing Proxy Services](#managing-proxy-services) to start the proxy is running.

<!---Vasily: Can we remove this? Maybe just a single sentence sending users to the Kubernetes integration??
### Install a Proxy on a Kubernetes Container

If you set up the Kubernetes integration, adding a proxy is part of the setup:
1. Log in to your Wavefront instance.
2. Click **Integrations** and click Kubernetes.
3. Click the **Setup** tab and follow the instructions to deploy a Wavefront proxy in Kubernetes and deploy Wavefront Collector for Kubernetes.

Depending on your environment, you might have to [customize proxy settings](proxies_configuring.html#configuring-a-proxy-in-a-container) for best performance.--->

### Scripted Proxy Installation

You can install a proxy using CLI or package install. Here's an overview with links:

* On Linux hosts, you can use the **[Wavefront CLI](wavefront_cli.html)** to install the Wavefront proxy and to perform certain management tasks.

* In Mac and Linux hosts, you can select the **integration for the host** and run only the command that installs the proxy, not the command that installs the Telegraf agent.

* To perform a **package install**, see [Installing a Proxy manually](proxies_manual_install.html). That page includes custom installation instructions for hosts with limited network connectivity.


<a name="restart"></a>

## Manage Proxy Services

After installing a proxy, you can start and stop the proxy service, check service status, and view the logs that are generated by the service. See [Logging](proxies_configuring.html#logging) for customizing your proxy, including its log configuration options.

### Start and Stop a Proxy

You can start and stop a proxy by running the following commands on the proxy host:

- Linux

  ```
  $ service wavefront-proxy [start | stop | restart]
  ```
- Mac

  ```
  $ brew services [start | stop | restart] wfproxy
  ```
- Docker

  ```
  $ docker [start | stop ] <proxy_container_id>
  ```
- Windows

  ```
  $ cd C:\Program Files (x86)\Wavefront\bin
  $ ./nssm.exe [start | stop] WavefrontProxy
  ```

### Check Proxy Service Status

To check if the proxy is running, run the following commands on the proxy host:

- Linux

  ```shell
  $ service wavefront-proxy status
  ```
  You can view the proxy log at `/var/log/wavefront/wavefront.log`.
- Mac

  ```shell
  $ brew services list
  ```
  You can view the proxy log at `/usr/local/var/log/wavefront/wavefront.log`.

- Windows

  ```
  $ cd C:\Program Files (x86)\Wavefront\bin
  $ ./nssm.exe status WavefrontProxy
  ```
  You can view the proxy log at `Program Files (x86)\Wavefront\wavefront.log`.

- Docker

  ```
  $ docker ps
  ```
  To view the proxy log, run `docker logs <proxy_container_id>`.

### Set the Listener Port for Metrics, Histograms, and Traces

The proxy listens on different ports for different kinds of data. These ports are specified in the [proxy configuration file](proxies_configuring.html#proxy-configuration-properties).

* For **metrics**, port 2878 is the default. To use a different port, set `pushListenerPorts`. explicitly.
* For **histograms**, set `histogramDistListenerPorts`.  See [Histogram Proxy Ports](proxies_histograms.html#histogram-proxy-ports) for details.
* For **trace data**, set `traceListenerPorts`.  The recommended port number is 30000.

<!---Vasily: is this belaboring the obvious?
{% include note.html content="If you are instrumenting your application with a Wavefront SDK to send data to the proxy, make sure the proxy's port settings match the port numbers you specify during SDK setup." %}
--->

### Test a Proxy

You can test that a proxy is receiving and sending data as follows:

1. Send data by running the following command:

   ```shell
echo -e "test.metric 1 source=test_host\n" | nc <wavefront_proxy_address> 2878
   ```
   where `<wavefront_proxy_address>` is the address of your Wavefront proxy.
1. In the Wavefront UI, select **Browse > Metrics**.
1. In the Metrics field, type `test.metric`.
1. Click `test.metric` to display a chart of the metric.

### Upgrade a Proxy

Wavefront frequently releases new proxy versions with new features. See the [Wavefront proxy github page](https://github.com/wavefrontHQ/java/releases) for details.

**Upgrade from the UI**

To upgrade from the UI, select **Browse > Proxies > Add New Proxy**. If an older version of the proxy exists, this process replaces it.

{% include note.html content="On Windows systems, you might have to uninstall the existing proxy first." %}

**Upgrade from the Command Line**

For Linux and Mac OS, can also upgrade a proxy from the command line as follows:

Linux| `sudo apt-get update && sudo apt-get install wavefront-proxy`
Linux (RPM)|`yum update wavefront-proxy`
Mac OS|`brew update && brew upgrade wfproxy`

### Uninstall a Proxy

When you upgrade a proxy, we uninstall the older version for you. You can also uninstall a proxy explicitly:

<table style="width: 100%;">
<tbody>
<thead>
<tr><th width="20%">OS</th><th width="80%">Instructions</th></tr>
</thead>
<tr>
<td markdown="span">Windows</td>
<td>The precise process depends on the version of Windows you're using. You follow the process for uninstalling programs. <ol><li>Click <strong>Start</strong>, and then click <strong>Control Panel</strong>.</li>
<li>Under Programs, click <strong>Uninstall a program</strong>.</li>
<li>Select <strong>Telegraf</strong> and click <strong>Uninstall</strong> at the top.</li>
<li>Select <strong>Wavefront Proxy</strong> and click <strong>Uninstall</strong> at the top.</li></ol></td></tr>
<tr><td>Linux</td>
<td><code>sudo apt-get remove wavefront-proxy
sudo apt-get remove telegraf</code></td></tr>
<tr><td>Linux (RPM)</td>
<td><code>sudo yum remove wavefront-proxy
sudo yum remove telegraf</code></td></tr>
<tr><td>Mac OS</td>
<td markdown="span">`bash -c "$(curl -s https://raw.githubusercontent.com/wavefrontHQ/homebrew-wavefront/master/sh/uninstall.sh)" `</td></tr>
</tbody>
</table>

### Export Data Queued at the Proxy

<!---Move this? It's kind of advanced, isn't it?--->

When you send too much data or if there is a network error, data starts to queue at the Wavefront proxy and create a backlog.

Use the following command to export the data that is queued at the proxy. Once the data is exported, you can make changes to the data, like removing the data that is not required, and send it back to the Wavefront proxy.

```
/opt/wavefront/wavefront-proxy/proxy-jre/java -jar /opt/wavefront/wavefront-proxy/wavefront-push-agent.jar --f /etc/wavefront/wavefront-proxy/wavefront.conf --exportQueuePorts <ports> --exportQueueOutputFile <outputFileNamePrefix> --exportQueueRetainData false
```
<table>
  <colgroup>
    <col width="25%"/>
    <col width="80%"/>
  </colgroup>
  <thead>
    <th>Parameter</th>
    <th>Description</th>
  </thead>
  <tbody>
    <tr>
      <td>
        <code>exportQueuePorts</code>
      </td>
      <td>
        Comma separated list of ports that listen to the data sent to the proxy. Can be a single port.
      </td>
    </tr>
    <tr>
      <td>
      <code>exportQueueOutputFile</code>
      </td>
      <td>
        Prefix you want the output files to have. If the prefix is wfproxy, the name of the file is wfproxy.&lt;FILE_NAME&gt;.txt
      </td>
    </tr>
    <tr>
      <td>
        <code>exportQueueRetainData</code>
      </td>
      <td markdown="span">
      When set to false, exports the data and removes the data from the backlog. Default is true.  <br/>
      {% include note.html content=" Make a backup of the files you export. If you set `exportQueueRetainData` to false, the exported files are the only copies you have of the backlog." %}
      </td>
    </tr>
  </tbody>
</table>

Example:

```
/opt/wavefront/wavefront-proxy/proxy-jre/java -jar /opt/wavefront/wavefront-proxy/wavefront-push-agent.jar --f /etc/wavefront/wavefront-proxy/wavefront.conf --exportQueuePorts 2878,3000 --exportQueueOutputFile wfproxy --exportQueueRetainData false
```

The example:
* Exports the data queued at ports 2878 and 3000.
* Creates output files that have the prefix wfproxy, such as wfproxy.points.2878.0.txt.
*	Deletes all data that’s currently in the proxy queue.


## Proxy Troubleshooting

<table>
<colgroup>
<col width="33%"/>
<col width="33%"/>
<col width="33%"/>
</colgroup>
<thead>
<tr>
<th>Error</th>
<th>Reason</th>
<th>Resolution</th>
</tr>
</thead>
<tbody>
<tr>
<td>You see "java: command not found" in <code>wavefront.log</code>.</td>
<td>Java is either not installed, or is not in your path.</td>
<td>Install Java using your local package manager, and make sure that your path includes the Java binary.</td>
</tr>
<tr>
<td>You see "Cannot fetch daemon configuration from remote server: org.jboss.resteasy.client.exception.ResteasyIOException: IOException" in <code>wavefront.log</code>.</td>
<td>You may have an incorrect server URL in your wavefront.conf file; you may have blocked the outgoing connection to that server URL (port 443); or the Wavefront servers may be down.</td>
<td>Run <code>curl &lt;wavefrontServerUrl&gt;</code> from the machine running the proxy, where <code>&lt;wavefrontServerUrl&gt;</code> is the full URL (including "https://) provided to you by Wavefront and in your <code>wavefront.conf</code> file.</td>
</tr>
<tr>
<td>You see "Cannot post work unit result to Wavefront servers. Will enqueue and retry later." in <code>wavefront.log</code>.</td>
<td>You may have an incorrect server URL in your <code>wavefront.conf</code> file; you may have blocked the outgoing connection to that server URL (port 443).</td>
<td>Run <code>curl &lt;wavefrontServerUrl&gt;</code> from the machine running the proxy, where <code>&lt;wavefrontServerUrl&gt;</code> is the full URL (including "https://") provided to you by Wavefront and in your <code>wavefront.conf</code> file.</td>
</tr>
<tr>
<td>You see "Exception in thread "main" java.lang.UnsupportedClassVersionError:
com/sunnylabs/GraphiteValidator : Unsupported major.minor version 51.0" in <code>wavefront.log</code>.
</td>
<td>You are using Java 1.6 or lower instead of Java 1.7.</td>
<td>Upgrade Java to 1.7 through your local package manager.</td>
</tr>
<tr>
<td>You see "Exception in thread "Thread-2" java.net.BindException: Address already in use" in <code>wavefront.log</code>.</td>
<td>You already have another process listening on port 2878, or may have started two proxies accidentally.</td>
<td>Use the <code>ps</code> command to find and delete any existing proxies, and then start the proxy again.</td>
</tr>
<tr>
<td>You can't run <code>telnet localhost 2878</code>; the connection is refused.</td>
<td>Ensure that you don't have an iptables rule blocking the traffic. Ensure that the proxy is running. Ensure that you are running <code>telnet localhost 2878</code> on the machine where the proxy is running.</td>
<td>Use the <code>ps</code> command to make sure that the proxy is running, and examine your iptables rules to ensure that TCP port 2878 is accessible locally.</td>
</tr>
</tbody>
</table>
