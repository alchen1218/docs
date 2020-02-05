---
title: Create and Customize Dashboards
tags: [getting started, dashboards, charts]
sidebar: doc_sidebar
permalink: ui_dashboards.html
summary: Create dashboards, add charts, and customize dashboard layout.
---

<table style="width: 100%;">
<tbody>
<tr>
<td width="80%">Wavefront dashboards allow you organize and customize the information about your environment. For example:
<ul>
<li>Organize charts into sections</li>
<li>Perform global operations such as setting the dashboard time window.</li>
<li>Use dashboard variables.</li></ul></td>
<td width="20%"><a href="ui_dashboards_v1.html"><img src="/images/classic_button.png" alt="click here for the classic doc"/></a></td>
</tr>
</tbody>
</table>

[Examine Data with Dashboards and Charts](ui_examine_data.html) explains how to set dashboard preferences, set the dashboard time window, isolate sources and series, and more.

{% include shared/badge.html content="Every Wavefront user can view dashboards and make some changes such as setting the time window. You must have Dashboard permission and Modify access to save changes you make to dashboards." %}

## Create a Dashboard

You have several options for creating a dashboard:

* Select **Dashboards > Create Dashboard**, drag in the Metrics or New Chart widget, and follow the wizard to create a single-chart or multi-chart dashboard.
* Select **Dashboards > Create Dashboard**, drag in the Templates widget, and select an integration, then pick the dashboards and charts you'd like to include.
* Select **Dashboards > All Dashboards** and click **Create Dashboard**
* Select **Browse > Metrics** and click **Create Dashboard**.

### Create a Dashboard from Metrics or Charts

It's easy to create a dashboard from metrics or by selecting a chart.

<table style="width: 100%;">
<tbody>
<tr>
<td width="40%">
<strong>To create a dashboard</strong>:
<ol><li>Select <strong>Dashboards > Create Dashboard</strong> from the task bar. </li>
<li>Drag the <strong>Metrics</strong> or <strong>New Chart</strong> widget onto the canvas</li>
<li>Select metrics, filters, and functions now or later. </li>
<li>In the top right, click <strong>Save</strong> and specify a name and URL for the dashboard. The URL field supports letters, numbers, underscores, and dashes.  The Name field supports letters, numbers, characters, and spaces.</li></ol></td>
<td width="60%"><img src="/images/v2_create_dashboard.png" alt="create dashboard"></td>
</tr>
</tbody>
</table>

### Create a Dashboard from Integration Templates

With release 2019.46, you can create a dashboard by specifying an integration dashboard as a template.

<table style="width: 100%;">
<tbody>
<tr>
<td width="40%">
<strong>To create a dashboard</strong>:
<ol><li>Select <strong>Dashboards > Create Dashboard</strong> from the task bar. </li>
<li>Drag the <strong>Templates</strong> widget onto the canvas. </li>
<li>Select first the source integration, then the dashboard you want as a template, and then one or more charts from that dashboard.</li>
<li>In the top right, click <strong>Save</strong> and specify a name and URL for the dashboard. The URL field supports letters, numbers, underscores, and dashes.  The Name field supports letters, numbers, characters, and spaces.</li></ol></td>
<td width="60%"><img src="/images/v2_create_dashboard_template.png" alt="create dashboard from template"></td>
</tr>
</tbody>
</table>

### Create a Dashboard from a Tracing Template

The Wavefront tracing dashboard includes a set of charts for monitoring the trace data sent by each service in your application. You can use this dashboard as a template and create a dashboard with these charts, then customize the charts for your environment

{% include note.html content="To view data in these charts, your applications need to send trace data to Wavefront. See [Instrument Your Applications](tracing_instrumenting_frameworks.html) for details." %}

<table style="width: 100%;">
<tbody>
<tr>
<td width="40%">
<strong>To create a dashboard</strong>:
<ol><li>Select <strong>Dashboards > Create Dashboard</strong> from the taskbar. </li>
<li>Drag the <strong>Tracing Templates</strong> widget onto the canvas. </li>
<li>Click <strong>Import Charts</strong>.</li>
<li>In the top right, click <strong>Save</strong> and specify a name and URL for the dashboard. 
  <ul>
  <li>The URL field supports letters, numbers, underscores, and dashes.</li> 
  <li>The Name field supports letters, numbers, characters, and spaces.</li>
  </ul>
</li>
<li>To view data that is specific to an application and service, use the <strong>application</strong> and <strong>service</strong> dropdowns.</li>
</ol></td>
<td width="60%"><img src="/images/create_tracing_template.png" alt="create dashboard from template"></td>
</tr>
</tbody>
</table>

**Take a look at the cool actions you can do using these charts:**
* [Navigate to the Tracing UI from the Service Metrics Dashboard](tracing_ui_overview.html#navigate-to-the-tracing-ui-from-the-service-metrics-dashboard). 
* [Explore the Default Service Metrics Dashboard](tracing_ui_overview.html#explore-the-default-service-metrics-dashboard).
* To delete or edit a chart, see [Clone, Delete, or Edit a Chart](#clone-delete-or-edit-a-chart).

## Edit or Clone a Dashboard

The dashboard menu allows you to create a dashboard, edit a dashboard, clone a dashboard, and look at the dashboard version history.

* When you **clone** a dashboard, you copy the dashboard. If you want to customize one of the Wavefront read-only dashboards, such as integration dashboards, just clone the dashboard and edit the clone.
* The dashboard  **version history** tracks each saved version and includes the user who saved the version. The result is an audit trail for the dashboard.

<table style="width: 100%;">
<tbody>
<tr>
<td width="40%">
<strong>To edit a dashboard</strong>:
<ol><li>Click the ellipsis icon in the top right of the dashboard. </li>
<li>Select <strong>Edit</strong> and make changes to the dashboard in edit mode.</li>
<li>Save the dashboard.</li></ol></td>
<td width="60%"><img src="/images/v2_edit_dashboard.png" alt="edit a dashboard"></td>
</tr>
<tr>
<td width="40%">
<strong>To clone a dashboard</strong>:
<ol><li>Click the ellipsis icon in the top right of the dashboard and select <strong>Clone</strong>. </li>
<li>Accept the suggested URL and dashboard name, or specify a new URL and dashboard name. Specify only the URL string. Do not include <code>https://</code>.  </li>
<li>Save the cloned dashboard.</li></ol></td>
<td width="60%"><img src="/images/v2_clone_dashboard.png" alt="clone a dashboard"></td>
</tr>
</tbody>
</table>

## Examine Metrics in Dashboard View Mode

All users can examine metrics, set the time window, and make temporary changes to dashboards. See [Examine Data](ui_examine_data.html) for details.

![dashboard elements](images/v2_dashboard_elements.png)

Here are some examples of what [all users can do](ui_examine_data.html):
* Set the dashboard time window
* Find a dashboard section
* Filter with global filters or dashboard variables
* Find a dashboard
* Isolate sources or series
* Fine-tune the time window


## Make Changes to a Dashboard in Edit Mode

When you create a dashboard or when you edit a dashboard, the dashboard is in Edit mode. In Edit mode, you can make several changes at a time, then save all changes to dashboard layout or to charts.

{% include shared/system_dashboard.html %}

<!---
To remove a change, click the revert icon to the left of **Edit JSON** on the task bar. The revert icon removes changes starting with the most recent and works backward. You can remove only changes made in the current Edit mode session.--->

![dashboard in edit mode](images/v2_dashboard_edit.png)

### Add a Chart Using Drag and Drop

<table style="width: 100%;">
<tbody>
<tr>
<td width="50%">
<ol><li>Drag and drop widgets onto the dashboard canvas </li>
<li>(Optional) Select metrics, filters, and functions.  </li>
<li>Scroll up and select <strong>Save</strong></li>
</ol></td>
<td width="50%"><img src="/images/v2_add_chart_wizard.png" alt="add chart wizard"></td>
</tr>
</tbody>
</table>

### Clone, Delete, or Edit a Chart

Editing a chart is different in View mode and in Edit mode:
* With a dashboard in View mode, click the chart title to edit a chart. <br>
* With a dashboard in Edit mode, use the icons on any chart.

<table style="width: 100%;">
<tbody>
<tr>
<td width="50%">
<ol><li>Place the cursor inside a chart. </li>
<li>Click one of the icons and follow the prompts. </li>
</ol>
</td>
<td width="50%"><img src="/images/v2_dashboard_edit_chart.png" alt="clone a chart"></td>
</tr>
</tbody>
</table>

### Add or Edit Dashboard Variables

<table style="width: 100%;">
<tbody>
<tr>
<td width="50%">
With the dashboard in Edit mode:
<ol>
<li>Scroll up to the Variables bar.  </li>
<li>Click the Edit icon to edit a variable.</li>
<li>Click the Add button to <a href="dashboards_variables.html">add a variable</a>.</li>
</ol>
</td>
<td width="50%">
<img src="images/v2_dashboard_variables.png" alt="select variables"></td>
</tr>
</tbody>
</table>

### Change Dashboard Layout

<table style="width: 100%;">
<tbody>
<tr>
<td width="50%">With the dashboard in Edit mode:
<ul><li>Rearrange charts within section or across sections (12 charts per row maximum). The grid determines what's possible.</li>
<li>Click the canvas to add a section. </li>
<li>Delete or move a section using the icons on the right of the section bar.</li>
</ul>
</td>
<td width="50%"><img src="/images/v2_add_section.png" alt="add section"></td>
</tr>
</tbody>
</table>


### Add a New or Cloned Chart

When you create a chart using **Dashboards > Create Chart**, you're prompted to save it to a dashboard. When you edit a chart, you can save to the current dashboard, or save a clone to a different dashboard.

<table style="width: 100%;">
<tbody>
<tr>
<td width="50%">
<ol><li>With the dashboard in View mode, create or edit a chart. </li>
<li>Select the <strong>v</strong> next to <strong>Save</strong> and make a selection:
<ul><li>To save to an existing dasbhoard, start typing the name of the dashboard, select a dashboard, and click <strong>Insert</strong></li>
<li>Click <strong>Save to New Dashboard</strong>, enter the dashboard name and URL, and click <strong>Create</strong>. Specify only the URL string; do not include https://. </li> </ul></li>
<li>When the target dashboard opens in Edit mode, click and drag the chart to the location of your choice and click <strong>Save</strong> at the top.</li></ol></td>
<td width="50%">
<img src="/images/v2_save_chart_to_new.png " alt="save to dashboard"></td>
</tr>
</tbody>
</table>

### Undo and Revert Undo Operations

Starting with release 2018.46.x, you can undo dashboard changes.

<table style="width: 100%;">
<tbody>
<tr>
<td width="40%">
<ol><li>Edit the dashboard and make one or more changes.</li>
<li>Click the icons to undo and to revert the undo. </li> </ol></td>
<td width="60%"><img src="/images/v2_undo.png" alt="Undo and redo icons"/></td>
</tr>
</tbody>
</table>

### Set Dashboard Display Preferences

For each dashboard, you can customize display preferences.

{% include tip.html content="To use the dark theme with your dashboard and all other Wavefront UI components, set your personal preferences [from the gear icon](users_account_managing.html#configuring-user-preferences)."%} 

<table style="width: 100%;">
<tbody>
<tr>
<td width="40%">
<strong>To set the dashboard display preferences</strong>:
<ol><li>Display a dashboard and select <strong>Edit</strong>. </li>
<li>Click <strong>Settings</strong>.</li>
<li>Make selections in the dialog:
<ul><li>Set the default time window. You can later override the time window.  </li>
<li>Hide the variables for the dashboard by default. Users can still show the variables bar using the icon.  </li>
</ul>
</li>
<li>Click <strong>Accept</strong>, and click <strong>Save</strong> </li>
<li>Optionally, you can display the <a href="events.html">Events</a> on charts using the Show Events dropdown.<br>
For more information on the options listed in the Show Events dropdown, see <a href="charts_events_displaying.html#controlling-event-overlays">Controlling Events Overlays</a>.</li>
</ol></td>
<td width="60%"><img src="/images/v2_dashboard_prefs.png" alt="set dashboard prefs"></td>
</tr>
</tbody>
</table>



{% include shared/system_dashboard.html %}
