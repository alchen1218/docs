---
title: Displaying Event Overlays in Charts
keywords: events
tags: [events, charts]
sidebar: doc_sidebar
permalink: charts_events_displaying.html
summary: Learn how to customize how events display in charts.
---

Examining events directly in charts can help you correlate metric anomalies with the events. This page explains how you can display an event *overlay* with details about the event, and customize the overlay.

## Event Overlay Example

The example below shows an event overlay that includes a link to the alert that generated it and other information about the event.

![Events queries](images/events_queries.png)

## Event Overlay Types and Color

Different events display in different ways:
* If a single event occurs in a time interval, the event icon displays as a **dot** on the chart's X-axis.
* If two or more events occur in a  time interval, the event icon displays as a **star** on the X-axis.
* Instantaneous events display a vertical line and non-instantaneous or ongoing events display a shaded region that represents the duration of the event. 

![Event overlay](images/event_overlay.png)

The event severity determines the color of the overlay:

-   **SEVERE** - red
-   **WARN** - orange
-   **SMOKE** - gray
-   **INFO** - blue

<a name="dashboards_events"></a>

## Control Event Overlays

Events can make your charts look cluttered. There are several options to control what’s shown on a chart:
* Options for individual charts
* Options that apply to the whole dashboard

### Configure Event Overlays for Individual Charts

There can be alert generated system events, or events that are manually created by a user. For details, see [Event Sources and Types](events.html#event-sources-and-types). You can configure a chart to show specific events.

<table style="width: 100%;">
  <tr>
    <th width="45%"> Action</th>
    <th width="55%"> Results</th>
  </tr>
  <tr>
    <td markdown="span">
      **Display events based on source events**:<br/> Select the **Display Source Events** checkbox in the [chart's' Format tab](ui_chart_reference.html#general) to display events related to alerts that fired for sources in the chart.
      <br/><br/>For example, if the query used in the chart uses metrics that have the application source tag, and these applications have events related to alerts that fired, you see these marked on the chart. See [Metric Data Format](wavefront_data_format.html#metrics) for details.
    </td>
    <td markdown="span">
      ![Select display source events](/images/events_display_source_events.png)
    </td>
  </tr>
  <tr>
    <td markdown="span">
      **Add an events() query to view specific events on a chart**:<br/> Add an [events() query](events_queries.html) to the chart. An `events()` query cannot be the only query on a chart. At least one `ts()` query must be enabled on the chart so that the `events()` query results display.
      <br/><br/>
      For example, `events(name="Latency Alert" and type="alert")` shows all the warning events on your chart.
    </td>
    <td markdown="span">
      ![Add an events query to the chart](/images/events_events_query_charts.png)
    </td>
  </tr>
</table>

### Specify an Events() Query for a Dashboard

<table style="width: 100%;">
  <tr>
    <th width="45%"> Action</th>
    <th width="55%"> Results</th>
  </tr>
  <tr>
    <td markdown="span">
       Set an [events() query](events_queries.html) in [dashboard preferences](ui_dashboards.html#set-dashboard-display-preferences), which you can access using the ellipsis menu in the top right corner of the dashboard.
       <br/><br/>
       For example, `events(name="High CPU Usage")` shows events that have the name `High CPU Usage` on all the charts of the dashboard.
    </td>
    <td markdown="span">
      ![Add an events query to the dashboard settings](/images/events_dashboard_settings_events_query.png)
    </td>
  </tr>
</table>

### View Events on Dashboards and Charts

Let's look at how you can view the events you added to the charts and dashboards in the above examples using the **Show Events** dropdown in the middle of the time bar.

<table style="width: 100%;">
  <tr>
    <th width="45%"> Show Events Options</th>
    <th width="55%"> Results</th>
  </tr>
  <tr>
    <td markdown="span">
       **From Chart**: Displays events based on the selection of the **Display Source Events** checkbox (default setting) and the `events()` query in a chart.
    </td>
    <td markdown="span">
      ![](/images/events_show_events_from_chart.png)
    </td>
  </tr>
  <tr>
    <td markdown="span">
       **From Dashboard Settings**: Displays events based on the `events()` query specified in dashboard settings. The source events are not displayed in this option.
    </td>
    <td markdown="span">
      ![](/images/events_show_events_dashboard_settings.png)
    </td>
  </tr>
  <tr>
    <td markdown="span">
       **From Chart & Dashboard**: Displays events based on the selection of the **Display Source Events** checkbox and the global `events()` query specified in dashboard settings.
    </td>
    <td markdown="span">
      ![](/images/events_show_events_charts_and_dashboards.png)
    </td>
  </tr>
  <tr>
    <td markdown="span">
       **Related Source Alerts**: Displays the events that were generated by alerts associated with sources of the charts.
    </td>
    <td markdown="span">
    ![](/images/events_show_events_related_source_alerts.png)
    </td>
  </tr>
  <tr>
    <td markdown="span">
       **All**: Displays all events that have occurred within the time window associated with the chart windows.
    </td>
    <td markdown="span">
      ![](/images/events_show_events_all.png)
    </td>
  </tr>
  <tr>
    <td markdown="span">
       **None**: Hides all events from every chart in the dashboard.
    </td>
    <td markdown="span">
      ![](/images/events_show_events_none.png)
    </td>
  </tr>
</table>
