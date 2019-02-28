---
title: Create and Customize Dashboards
tags: [getting started, dashboards, charts]
sidebar: doc_sidebar
permalink: ui_dashboards.html
summary: Organize charts into dashboards.
---
Wavefront dashboards allow you to organize charts into sections, perform global operations such as [setting the dashboard time window](ui_examine_data.html#set-the-dashboard-time-window) and use [dashboard variables](dashboards_variables.html). Watch this video for an introduction to dashboards:

<p><a href="https://vmwarelearningzone.vmware.com/oltpublish/site/openlearn.do?dispatch=previewLesson&id=5103bc38-dc7a-11e7-a6ac-0cc47a352510&inner=true&player2=true"><img src="/images/v_dashboards_intro.png" style="width: 700px;" alt="introduction to dashboards"/></a>
</p>

While every Wavefront user can view dashboards, you must have Dashboard permission to save changes you make to dashboards.

## Create a Dashboard

You can create a dashboard from the All Dashboards page, from a chart, from the Metrics browser, or by cloning an existing dashboard.

1. Start dashboard creation:
   <table style="width: 100%;">
   <tbody>
   <thead>
   <tr><th width="20%">First...</th><th width="80%">Then...</th></tr>
   </thead>
   <tr><td markdown="span">Click **Browse > All Dashboards**</td>
   <td markdown="span">Click the **Create Dashboard** button on the filter bar.</td></tr>
   <tr><td markdown="span">Select a dashboard</td>
   <td markdown="span">Click the pencil icon on the right side of the browser window below the task bar, and select **New**. Or click **Clone** to create a new dashboard based on the existing dashboard you are currently viewing.</td></tr>
   <tr><td markdown="span">Click **Browse > Metrics**</td>
   <td markdown="span">click **Create Dashboard** in the top left corner of the page to begin creating a dashboard. Creating a dashboard this way automatically creates sections and charts based on the current set of metrics in the Metrics list. Sections are created by first-level nodes, individual charts by second-level nodes and finally all metrics on the third-level or below are rendered on a single chart. Creating a dashboard from the Metrics browser allows you to apply additional parameters such as sources and source tags.</td></tr></tbody>
   </table>
2. In the Create New Dashboard dialog, enter a URL and name for your dashboard. The URL field supports letters, numbers, underscores, and dashes.  The Name field supports letters, numbers, characters, and spaces.

   **Note** If you enter any special character or space, then the URL field turns red and requires you to make changes before saving. You can specify `my_url` but not `http://my_url.com` because `://` and the period are special characters.

3.  Click **Create**, edit the dashboard, and click **Save**.

**Note:** Many Wavefront customers use existing dashboards as templates, and store dashboard templates as well as dashboards generated from scratch in a a repository such as GitHub.

## Organize Dashboards with Sections

Dashboard sections allow you to group saved charts. Every dashboard has at least one section. The section link bar is located directly below the time bar.

![section toc](images/section_links.png)

Here's how you can organize your charts with sections:

<table style="width: 100%;">
<tbody>
<thead>
<tr><th width="20%">Task</th><th width="80%">Process</th></tr>
</thead>
<tr><td markdown="span">Create a section</td>
<td markdown="span">Click **New Section** below the time bar. </td></tr>
<tr><td markdown="span">Change the section name</td>
<td markdown="span"> </td></tr>
<tr><td markdown="span">Jump to a section</td>
<td markdown="span">Clicking the link in the section bar. If the section link bar is not displayed, turn it on in the Dashboard Display Preferences.</td></tr>
<tr><td markdown="span">Resize rows</td>
<td markdown="span">Clicking the resize icons <img src="images/row_resize.png#inline"  alt="resize rows"/> on the left of each row. Resizing a row resizes every chart in that row.</td></tr></tbody>
</table>

## Add a Chart to a Dashboard

You can add newly-created charts and existing charts to a dashboard.
* When you create a chart, you can save it to an existing dashboard or select **Save to New Dashboard**
* You can also add existing charts to a dashboard. You can build your own dashboards by reusing charts from other dashbords. You can do that by cloning a chart and adding it to a new dashboard. We don't remove the selected chart from its current dashboard, add the clone to the new or existing dashboard:

1. Click the chart name in the upper-right corner of the chart you want to add.

    ![chart_title](images/chart_title.png)

1. Scroll down to the Save To section and click **Choose Dashboard**.
1. Start entering the dashboard name and select the target dashboard from the dropdown list and click **Insert**.
   The selected dashboard opens in edit mode and the chart you selected displays at the top of the dashboard in an **Incoming Chart** section.
1. Click and drag the chart to the location of your choice and click **Save** at the top of the dashboard browser.

## Clone a Dashboard

1. To clone a dashboard:
  -   From the All Dashboards page, locate the dashboard, click the three dots, and select Clone.
  ![clone dashboard](images/dashboard_clone.png)
  -   From a dashboard, click the pencil icon on the right side below the task bar, and select **Clone**.
1. Specify a new URL. The URL field supports letters, numbers, underscores, and dashes. Do not include `https://`
1.(Optional) Edit the dashboard name.
1. Click **Clone**.

## Edit a Dashboard

{% include shared/system_dashboard.html %}

When you edit a dashboard, you can make changes to the dashboard description, name, [dashboard variables](dashboards_variables.html), sections, and charts.

1. Put the dashboard into Edito mode:
-   From the All Dashboards page, locate the dashboard, click the three dots on the left and and select **Edit**.
![edit dashboard](images/dashboard_edit.png)
-   From a dashboard, click the pencil icon on the right side below the task bar, and select **Edit**.
1. Edit the dashboard.

   When you edit a dashboard, you may make several changes at a time. To remove a change, click the revert icon to the left of **Edit JSON** on the task bar. The revert icon removes changes starting with the most recent and works backwards. You can remove only changes made in the current edit mode session.
2. To save configuration changes, click the  **Save** button in the top right.

   ![save dashboard](images/save_dashboard.png)
