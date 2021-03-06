---
title: Manual Wavefront Proxy Install on Linux
keywords: Ansible
tags: [proxies, best practice]
sidebar: doc_sidebar
permalink: proxies_manual_install.html
summary: Learn how to manually install a Wavefront proxy and Telegraf agent.
---
Most Wavefront customers perform a [scripted installation](proxies_installing.html#proxy-installation) of the Wavefront proxy and Telegraf agent. In some environments, it's necessary to perform a manual installation instead. This page gives some guidance. You can perform additional customization using [proxy configuration properties](proxies_configuring.html#general-proxy-properties-and-examples).

{% include note.html content="Because the exact steps depend on your environment, this page can't give details for each use case. [Advanced Proxy Configuration](proxies_configuring.html) gives details about settings you can change in the proxy config file." %}

## Proxy Install - Full Network Access

Follow these steps to install a proxy on a host with full network access (incoming and outgoing connections).

### Prerequisites

Before you begin the installation process, [test connectivity](proxies_manual_install.html#testing-proxy-host-connectivity) between the host being configured and your Wavefront service.


### Step 1: Download the Proxy

If your system accepts incoming traffic, you can download the proxy file as follows:

1. Download the proxy `.rpm` or `.deb` file from [packagecloud.io/wavefront/proxy](http://packagecloud.io/wavefront/proxy).
2. Run `sudo rpm -U <name_of_file.rpm>` or `sudo dpkg -i <name_of_file.deb>`.
    {% include note.html content="<br/>If no Java JRE is in the path, this command installs JRE locally under `/opt/wavefront/wavefront-proxy/proxy-jre`." %}

### Step 2: Determine Proxy Settings

Before you can customize the proxy configuration, you have to find the values for your environment. You need the following information to customize the settings.

{% include note.html content="To find the values for server and token, you can select **Integrations** from the taskbar, select **Linux Host**, and select the **Setup** Tab."%}

<table style="width: 100%;">
<tbody>
<thead>
<tr><th width="20%">Parameter</th><th width="60%">Description</th><th width="20%">Example</th></tr>
</thead>
<tr>
<td markdown="span">**server**</td>
<td>URL of the Wavefront server you log in to.  </td>
<td>https://try.wavefront.com/api/ </td>
</tr>
<tr>
<td markdown="span">**token**</td>
<td markdown="span">API token. See **Note** above.</td>
<td>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx </td>
</tr>
<tr>
<td markdown="span">**hostname**</td>
<td markdown="span">Name of the host the proxy will run on. The hostname is not used to tag your data; rather, it's used to tag data internal to the proxy, such as JVM statistics, per-proxy point rates, and so on. Alphanumeric and periods are allowed. </td>
<td>cust42ProxyHost</td>
</tr>
<tr>
<td markdown="span">**enable graphite**</td>
<td markdown="span">Whether to enable the Graphite format. See the [Graphite integration](graphite.html) for details on Graphite configuration.  </td>
<td>cust42ProxyHost</td>
</tr>
<tr>
<td markdown="span">**tlsPorts**</td>
<td markdown="span">Comma-separated list of ports to be used for incoming HTTPS connections. </td>
</tr>
<tr>
<td markdown="span">**privateKeyPath**</td>
<td markdown="span">Path to PKCS#8 private key file in PEM format. Incoming HTTPS connections access this private key. </td>
</tr>
<tr>
<td markdown="span">**privateCertPath**</td>
<td markdown="span">Path to X.509 certifivate chain file in PEM format. Incoming HTTPS connections access this certificate. </td>
</tr>
</tbody>
</table>

### Step 3: Make Configuration Changes

You can make configuration changes by editing the config file or by running a script.

#### Option 1: Editing the Config File

If you want to edit the configuration file manually:

1. Find, uncomment and modify the configuration:
   <table>
   <tbody>
   <thead>
   <tr><th width="20%">Change to Make</th><th width="80%">Config Parameters</th></tr>
   </thead>
   <tr>
   <td markdown="span">Change target. </td>
   <td>
   <code>server=
   hostname=
   token=
   </code>
   </td></tr>
   <tr>
   <td markdown="span">Use Graphite</td>
   <td>If you want to use Graphite, specify the Graphite configuration section starting with:
   <code>graphitePorts=2003
   </code>
   </td></tr>
   </tbody>
   </table>

2. Start the Wavefront proxy service:
   ```
   sudo service wavefront-proxy start
   ```

#### Option 2: Running the autoconf Script

You can specify the settings by running `bin/autoconf-wavefront-proxy.sh`.

After the interactive configuration is complete:
* The Wavefront proxy configuration at `/etc/wavefront/wavefront-proxy/wavefront.conf` is updated with the input that you provided.
* The `wavefront-proxy` service is started.


## Proxy Install -- Limited Network Access

Some Wavefront customers want to run the proxy on a host with limited network access.

### Prerequisites

- **Networking:** The minimum requirement is an outbound HTTPS connection to the Wavefront service so the proxy can send metrics to the Wavefront service.
  For metrics, the proxy uses port 2878 by default. You change that and you can configure [additional proxy ports](proxies_installing.html#configuring-proxy-ports-for-metrics-histograms-and-traces) for histograms and traces.

  You can use an [HTTP proxy](proxies_manual_install.html#connecting-to-wavefront-through-an-http-proxy) for the connection.

- **JRE:** The Wavefront proxy is a Java jar file and requires a JRE - for example, openjdk8.  If the JRE is in the execution path you should be able to install the .rpm or .deb file as above.

### Installation and Configuration

Installation and configuration is similar to environments with full network access but might require additional work.

1. Make sure all prerequisites are met, including an open outgoing HTTPS connection to the Wavefront service and JRE.
2. Install the .rpm or .deb file.
3. Update the settings, either by editing the configuration file or by running the autoconf script, as explained above.
4. You may need to update the Wavefront control file  `/etc/init.d/wavefront.proxy` to the following settings:

```
   desc=${DESC:-Wavefront Proxy}
   user="wavefront"
   wavefront_dir="/opt/wavefront"
   proxy_dir=${PROXY_DIR:-$wavefront_dir/wavefront-proxy}
   config_dir=${CONFIG_DIR:-/etc/wavefront/wavefront-proxy}
   proxy_jre_dir="$proxy_dir/proxy-jre"                  <-- set to location of currently installed JRE
   export JAVA_HOME=${PROXY_JAVA_HOME:-$proxy_jre_dir}   <-- set to location of currently installed JRE
```

## Proxy Custom Install with Incoming TLS/SSL

By default Wavefront proxy can accept incoming TCP and HTTP requests on the port specified by `pushListenerPorts`. You can also configure the proxy to accept only connections with a certificate and key.

In that case:
1. Specify that you want to open the port with the `pushListenerPorts` config parameter.
2. Specify the `tlsPorts`, `privateKeyPath`, and `privateCertPath` parameters.

The following parameters support TLS/SS. You can specify those parameters in the configuration file or by running `bin/autoconf-wavefront-proxy.sh`, as discussed above.

<table style="width: 100%;">
<tbody>
<thead>
<tr><th width="20%">Parameter</th><th width="80%">Description</th></tr></thead>
<tr>
<td markdown="span">**tlsPorts**</td>
<td markdown="span">Comma-separated list of ports to be used for incoming TLS/SSL connections. </td>
</tr>
<tr>
<td markdown="span">**privateKeyPath**</td>
<td markdown="span">Path to PKCS#8 private key file in PEM format. Incoming TLS/SSL connections access this private key. </td>
</tr>
<tr>
<td markdown="span">**privateCertPath**</td>
<td markdown="span">Path to X.509 certifivate chain file in PEM format. Incoming TLS/SSL connections access this certificate. </td>
</tr>
</tbody>
</table>

## Testing Proxy Host Connectivity

You can test connectivity from the proxy host to the Wavefront server using curl.

Run this test before installing the proxy, and again after installing and configuring the proxy.

1. Find the values for server and token:
   1. Select **Integrations** from the taskbar.
   2. Select **Linux Host** and click the **Setup** tab.
2. Run the following command:
   ```
   curl -v https://your-server.wavefront.com/api/daemon/test?token=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
   ```

Here is an example of the expected return when you use the `-v` parameter (without `-v` only HTTP(S) errors are reported):
```
* About to connect() to myhost.wavefront.com port 443 (#0)
*   Trying NN.NN.NNN.NNN...
* Connected to myhost.wavefront.com (NN.NN.NNN.NNN) port 443 (#0)
* Initializing NSS with certpath: sql:/etc/pki/nssdb
*   CAfile: /etc/pki/tls/certs/ca-bundle.crt
  CApath: none
* SSL connection using TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256
* Server certificate:
* 	subject: CN=*.wavefront.com,O="VMware, Inc",L=Palo Alto,ST=California,C=US
* 	start date: <date>
* 	expire date: <date>
* 	common name: *.wavefront.com
* 	issuer: <issuer details>
> GET /api/daemon/test?token=<mytoken> HTTP/1.1
> User-Agent: curl/7.29.0
> Host: myhost.wavefront.com
> Accept: */*
>
< HTTP/1.1 200 OK
< Server: nginx
< Date: Wed, 09 Jan 2019 20:41:45 GMT
< Transfer-Encoding: chunked
< Connection: keep-alive
< X-Upstream: 10.15.N.NNN>:NNNNN
< X-Wavefront-Cluster: /services-<services>
< X-Frame-Options: SAMEORIGIN
<
* Connection #0 to host myhost.wavefront.com left intact
```

## Testing Your Installation

After you have started the proxy you just configured, you can verify its status from the Wavefront UI or with curl commands.

### Testing From the UI
To check your proxy from the UI:
1. Log in to Wavefront from a browser.
2. From the taskbar, select **Browse > Proxies** to view a list of all proxies.
   If the list is long, type the proxy name as defined in `hostname=` in  `wavefront.conf` to located the proxy by name.

### Testing Using curl

You can test your proxy using `curl`. Documentation for the following curl commands can be found directly on your Wavefront server at `https://<your-server.wavefront.com>/api-docs/ui/#!/Proxy/getAllProxy`.

You can run the commands [directly from the API documentation](https://tanzu.vmware.com/content/vmware-tanzu-observability-blog/did-you-know-that-our-api-docs-are-alive). This is less error prone than copy/paste of the token.

For this task, you first you get the list of proxies for your Wavefront service, then you display information for just the proxy you installed.

Step 1: Get the list of proxies for your Wavefront server:
```
curl -X GET --header "Accept: application/json" --header "Authorization: Bearer xxxxxxxxx-<your api token>-xxxxxxxxxxxx" "https://<yourserver.wavefront.com/api/v2/proxy?offset=0&limit=100"
```

This command returns a JSON formated list of all proxies.
Step 2: Get the proxy ID. You can search the output using the proxy name configured in `wavefront.conf`, or find the proxy ID in the UI.

Step 3: Return information for only this proxy:
```
curl -X GET --header "Accept: application/json" --header "Authorization: Bearer xxxxxxxxx-<your api token>-xxxxxxxxxxxx" "https://<yourserver>.wavefront.com/api/v2/proxy/443e5771-67c8-40fc-a0e2-674675d1e0a6"
```

Sample output for single proxy:

```
{
    "status": {
        "result": "OK",
        "message": "",
        "code": 200
    },
    "response": {
        "customerStatus": "ACTIVE",
        "version": "4.34",
        "status": "ACTIVE",
        "customerId": "mike",
        "inTrash": false,
        "hostname": "mikeKubeH",
        "id": "443e5771-67c8-40fc-a0e2-674675d1e0a6",
        "lastCheckInTime": 1547069052859,
        "timeDrift": -728,
        "bytesLeftForBuffer": 7624290304,
        "bytesPerMinuteForBuffer": 31817,
        "localQueueSize": 0,
        "sshAgent": false,
        "ephemeral": false,
        "deleted": false,
        "statusCause": "",
        "name": "Proxy on mikeKubeH"
    }
}
```

## Connecting to Wavefront Through an HTTP Proxy

The Wavefront proxy initiates an HTTPS connection to the Wavefront service. The connection is made over the default https port 443. Connection parameters are configured in `/etc/wavefront/wavefront-proxy/wavefront.conf`.

You can instead configure the HTTP proxy setting within `wavefront.conf`.

{% include note.html content="The Wavefront proxy does not use the `http_proxy` environmental variables. You must specify the information in `wavefront.conf`." %}

### Using curl to Test HTTP Proxy Connections

If your environment requires the use of an HTTP proxy, add the proxy configuration command line directives or have the HTTP_PROXY environmental variable set.  These parms are the same as the HTTP proxy related configuration values need in `wavefront.conf`.

For curl testing, here are examples of setting the `http_proxy` environment variables. These variables are not used by the proxy itself.

```
Set these variables to configure Linux proxy server settings for the command-line tools:

$ export http_proxy="http://PROXY_SERVER:PORT"
$ export https_proxy="https://PROXY_SERVER:PORT"
$ export ftp_proxy="http://PROXY_SERVER:PORT"
If a proxy server requires authentication, set the proxy variables as follows:

$ export http_proxy="http://USER:PASSWORD@PROXY_SERVER:PORT"
$ export https_proxy="https://USER:PASSWORD@PROXY_SERVER:PORT"
$ export ftp_proxy="http://USER:PASSWORD@PROXY_SERVER:PORT"
```

### Modifying wavefront.conf for HTTP Connections

By default, the HTTP proxy section is commented out. Uncomment and configure with values required by your HTTP proxy:

```
## The following settings are used to connect to Wavefront servers through a HTTP proxy:
#proxyHost=localhost
#proxyPort=8080
## Optional: if http proxy requires authentication
#proxyUser=proxy_user
#proxyPassword=proxy_password
#
```

You can then follow the other Wavefront proxy setup steps.

## Installing Telegraf Manually

If the system has network access, follow our [instructions for installing Telegraf](https://packagecloud.io/wavefront/telegraf/install)

We include instructions for using .deb, .rpm, node, python, and gem for installing from the network.

If you're in an environment with restricted network access:
1. Download the appropriate Telegraf package [from InfluxData](https://portal.influxdata.com/downloads/) and install as directed.
2. Create a file called `10-wavefront.conf` in `/etc/telegraf/telegraf.d` and enter the following snippet:
```
[[outputs.wavefront]]
  host = "WAVEFRONT_PROXY_ADDRESS"
  port = 2878
  metric_separator = "."
  source_override = ["hostname", "agent_host", "node_host"]
  convert_paths = true
```
3. Start the Telegraf agent:

```
   sudo service telegraf start
```
