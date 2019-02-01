---
title: Wavefront Observability SDKs for Java
keywords: data, distributed tracing
tags: [tracing]
sidebar: doc_sidebar
permalink: wavefront_sdks_java.html
summary: Find Wavefront SDKs for instrumenting a Java application to send observability data to Wavefront.
---


This page lists the available [Wavefront observability SDKs](wavefront_sdks.html) for collecting metrics, histograms, and trace data from the microservices in a Java application. 

To obtain an SDK, click on the link and follow the setup steps on GitHub. 

* **Note:** Be sure to use the latest version of the SDK.

## Framework-level Java SDKs

Each [framework-level SDK](wavefront_sdks.html#sdks-for-instrumenting-application-frameworks) collects observability data from a particular Java framework or component, with minimal code setup.

<table id = "framework-java" width="100%">
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<tbody>
<thead>
<tr><th>Wavefront SDK</th><th>Description</th><th>Observability Data</th></tr>
</thead>

<tr>
<td markdown="span">[Dropwizard Jersey SDK for Java](https://github.com/wavefrontHQ/wavefront-jersey-sdk-java)</td>
<td>Instruments Dropwizard, a Jersey-compliant framework for building RESTful Web services. Sends observability data from HTTP requests and responses.</td>
<td markdown="span">Metrics, histograms, trace data</td>
</tr>

<tr>
<td markdown="span">[gRPC SDK for Java](https://github.com/wavefrontHQ/wavefront-gRPC-sdk-java)</td>
<td>Instruments gRPC, a framework for building services that communicate through remote procedure calls. Sends observability data from gRPC requests and responses.</td>
<td markdown="span">Metrics, histograms, trace data</td>
</tr>

<tr>
<td markdown="span">[JAX-RS SDK for Java](https://github.com/wavefrontHQ/wavefront-jaxrs-sdk-java)</td>
<td>Instruments a JAX-RS (JSR 311: The Java API for RESTful Web Services) implementation for building RESTful Web services. Sends observability data from HTTP requests and responses.</td>
<td markdown="span">Metrics, histograms, trace data</td>
</tr>

<tr>
<td markdown="span">[JVM SDK](https://github.com/wavefrontHQ/wavefront-runtime-sdk-jvm)</td>
<td>Instruments the Java Virtual Machine to send runtime metrics and histograms to Wavefront. Sends observability data for CPU usage, disk usage, and so on.</td>
<td markdown="span">Metrics, histograms</td>
</tr>

<tr>
<td markdown="span">[Spring Boot Jersey SDK for Java](https://github.com/wavefrontHQ/wavefront-jersey-sdk-java)</td>
<td>Instruments Spring Boot, a Jersey-compliant framework for building RESTful Web services. Sends observability data from HTTP requests and responses.</td>
<td markdown="span">Metrics, histograms, trace data</td>
</tr>

</tbody>
</table>

## Custom-level Java SDKs

Each [custom-level SDK](wavefront_sdks.html#sdks-for-instrumenting-custom-operations) enables you to instrument critical-path, proprietary business operations that are not based on an instrumented framework. You'll need to add some code to each operation that is to report observability data.

<table id = "custom-java" width="100%">
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<tbody>
<thead>
<tr><th>Wavefront SDK</th><th>Description</th><th>Observability Data</th></tr>
</thead>
<tr>
<td markdown="span">[Dropwizard Metrics SDK for Java](https://github.com/wavefrontHQ/wavefront-dropwizard-metrics-sdk-java)</td>
<td>Implements Dropwizard Metrics, so you can instrument custom business operations to collect and send metrics and histograms to Wavefront. </td>
<td markdown="span">Metrics, histograms</td>
</tr>
<tr>
<td markdown="span">[OpenTracing SDK for Java](https://github.com/wavefrontHQ/wavefront-opentracing-sdk-java)</td>
<td markdown="span">Implements the [OpenTracing](https://www.opentracing.io) specification, so you can instrument custom business operations to collect and send traces and spans to Wavefront. </td>
<td markdown="span">Trace data</td>
</tr>
</tbody>
</table>

## Core Java SDK

The [core SDK](wavefront_sdks.html#core-sdks-for-sending-raw-data-to-wavefront) enables you send raw values to Wavefront for ingestion as metrics, histograms, or trace data. 

<table id = "core-java" width="100%">
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<tbody>
<thead>
<tr><th>Wavefront SDK</th><th>Description</th><th>Observability Data</th></tr>
</thead>
<tr>
<td markdown="span">[Core SDK for Java](https://github.com/wavefrontHQ/wavefront-sdk-java)</td>
<td>Sends raw data values either directly to the Wavefront service or to a Wavefront proxy. </td>
<td markdown="span">Metrics, histograms, trace data</td>
</tr>

</tbody>
</table>