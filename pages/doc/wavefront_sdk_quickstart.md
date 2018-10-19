---
title: Instrumenting Your App for Tracing
keywords: data
tags: [tracing]
sidebar: doc_sidebar
permalink: wavefront_sdk_quickstart.html
summary: Learn how to set up your application to send data into Wavefront.
---

One of the ways to start the flow of traces into Wavefront is to instrument your application. Instrumentation enables you to trace a transaction flow from end to end across multiple distributed services, guided by key metrics from your application. By displaying a transaction as a hierarchy of Wavefront tracing _spans_, you can pinpoint where the transaction is spending most of its time, and discover where it might be failing.

This page shows you the fast track to producing out-of-the-box metrics and tracing spans from popular application frameworks. 

**Note:** You can also instrument your application to send custom traces and metrics. _[[Link to other SDKs]]_

## Sample Setup

Watch this video to see how to set up a sample application to send out-of-the-box metrics and traces. (You can read about the steps [below](#setup-process).)

_[[video that describes how to set up BeachShirts app]]_

## Setup Process 

You get the exact setup steps by picking a programming language and framework in a [table below](#pick-a-language-and-framework-to-instrument). 

In all cases, you will:
 
1. Add dependencies in the build system of your choice, such as Maven. 

2. Edit your code to instantiate a few helper objects. These objects:
  * [Specify how to send data](#configuring-how-to-send-data-to-wavefront) -- through a Wavefront proxy or directly to the Wavefront service.
  * [Describe your application to Wavefront](#describing-your-application-to-wavefront). 
  * [Configure how frequently data is reported](#configuring-data-reporting). 


3. Start a Wavefront proxy if you are using one. 

After your application starts running, you can click **Applications** in the Wavefront menu bar to start exploring your metrics and traces.


## Pick a Language and Framework to Instrument 

Pick the language and framework used by the service you want to instrument. Click on the link to go to the detailed setup steps.

<table width="100%">
<colgroup>
<col width="20%" />
<col width="80%" />
</colgroup>
<tbody>
<thead>
<tr><th>Java Framework</th><th>Description</th></tr>
</thead>
<tr><td markdown="span">[Jersey Compliant](https://github.com/wavefrontHQ/wavefront-jersey-sdk-java)</td>
<td>Instruments all Jersey-compliant APIs to send telemetry data to Wavefront, such as DropWizard and Spring Boot.</td></tr>
<tr><td markdown="span">gRPC</td>
<td>Instruments all gRPC APIs to send telemetry data to Wavefront.</td></tr>
<tr><td markdown="span">JVM</td>
<td>Instruments Java Virtual Machine calls to send metrics and histograms to Wavefront. Measures CPU, disk usage, and so on.</td></tr>
</tbody>
</table>

<table width="100%">
<colgroup>
<col width="20%" />
<col width="80%" />
</colgroup>
<tbody>
<thead>
<tr><th>C#/.NET Framework</th><th>Description</th></tr>
</thead>
<tr><td markdown="span"> TBD </td>
<td>TBD</td></tr>
<tr><td markdown="span">TBD</td>
<td>TBD</td></tr>
</tbody>
</table>


_[[Links from this table should be either go to the GitHub readme.md file, or to a doc page generated from that file.]]_

## Describing Your Application to Wavefront

Part of instrumenting an application framework is to specify values for a few tags that describe the architecture of your application as it is deployed. These tags (called _application metadata_) will be associated with the data sent from each operation that uses an API from the instrumented framework. Wavefront uses these tags for aggregating predefined metrics that provide you with a meaningful context for your application's traces. 

In your application, you instantiate an _application tags_ object that will store your values for the metadata tags.
Because the metadata tags describe the application's architecture as it is deployed, you typically implement a mechanism for obtaining tag values from a configuration file, which you then update for each deployed application instance.

For each microservice that uses an instrumented framework, you specify the following required tags:
* `application` - Name that identifies the application. If the application is composed of coordinated microservices, all of those microservices should share the same application name.
* `service` - Name that identifies the microservice. Each microservice should have its own service name.

If the physical topology of your application will be useful for filtering metrics, you can specify the following optional tags:
* `cluster` - Name of a group of related hosts that serves as a cluster or region in which the application will run. 
* `shard` - Name of a subgroup of hosts within a cluster that serve as a partition or replica.

**Note:** For details, see _[[link to tagging topic on another page]]_.

## Configuring How to Send Data to Wavefront

Part of instrumenting an application framework is to specify how you want metrics and spans to be sent to Wavefront. The recommended way in most cases is to send data to a Wavefront proxy, which in turn forwards the data to the Wavefront service. An alternative is for your applications to send data directly to the Wavefront service.

* If you choose to use a proxy, you will need to specify the ports it listens to for metrics, histograms, and tracing data. 
* If you choose direct ingestion, you can optionally change the defaults for batching up the data to be sent. 

In either case, you can optionally tune performance by changing the default frequency for flushing data into Wavefront.

In your application, you instantiate a _Wavefront sender_ object that will store your ingestion choices.
To make it easy to reconfigure the sender at runtime, you typically implement a mechanism for obtaining values from a configuration file.
You only need one Wavefront sender object per JVM. If you are instrumenting multiple frameworks in the same JVM, they should all share the same Wavefront sender object.

**Note:** For details, see _[[link to proxy vs direct ingest topic on another page]]_.

## Configuring Data Reporting
<!--- Mention source here? --->

Part of instrumenting an application framework is to specify a reporting interval, which determines the timestamps of data points sent to Wavefront. The default reporting interval is once a minute. (The reporting interval controls how often data is reported to the Wavefront sender, which has a separate interval for sending the reported data on to the Wavefront service.) 
 
In your application, you instantiate a _Wavefront reporter_ object that will store your reporting interval.
To make it easy to reconfigure the sender at runtime, you normally implement and use a mechanism for obtaining values from a configuration file.

**Note:** For details, see _[[link to reporting interval topic on another page]]_.