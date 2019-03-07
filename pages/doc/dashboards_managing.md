---
title: Managing Dashboards
keywords: dashboards
tags: [dashboards]
sidebar: doc_sidebar
permalink: dashboards_managing.html
summary: Learn how to manage dashboards.
---

{% include shared/permissions.html entity="dashboards" entitymgmt="Dashboard" %}

## Creating a Dashboard

You can create a dashboard from the All Dashboards page, an existing dashboard, a chart, or the Metrics browser.

1.  Do one of the following:

    -   In the All Dashboards page, click the **Create Dashboard** button on the filter bar.
    -   From an existing dashboard, click the pencil icon on the right side of the browser window below the task bar, and select **New**. Or click **Clone** to create a new dashboard based on the existing dashboard you are currently viewing.
    -   From a chart, scroll down to the **Save to** section and click **New Dashboard**.
    -   From the Metrics browser, click **Create Dashboard** in the top left corner of the page to begin creating a dashboard. Creating a dashboard this way automatically creates sections and charts based on the current set of metrics in the Metrics list. Sections are created by first-level nodes, individual charts by second-level nodes and finally all metrics on the third-level or below are rendered on a single chart. Creating a dashboard from the Metrics browser allows you to apply additional parameters such as sources and source tags.

    A Create New Dashboard dialog displays. Enter a URL and name for your dashboard.

    * The URL field supports letters, numbers, underscores, and dashes.
      **Note** If you enter any special character or space, then the URL field turns red and requires you to make changes before saving. You can specify `my_url` but not `http://my_url.com` because `://` and the period are special characters.
    * The Name field supports letters, numbers, characters, and spaces.

2.  Click **Create**, edit the dashboard, and click **Save**.

**Note:** Many Wavefront customers manage use existing dashboards as templates, and store dashboard templates as well as dashboards generated from scratch in a a repository such as GitHub.

## Deploying a Dashboard

Dashboards are stored in JSON format. You can deploy a dashboard JSON file with the [Wavefront API](wavefront_api.html) using the following command:

```shell
curl -v https://<wavefront_instance>.wavefront.com/api/v2/dashboard -d @<dashboard_file>.json \
  -H "Content-Type: application/json" -H 'Authorization: Bearer <wavefront_api_token>'
```

<span id="prefs"/>

## Setting Dashboard Preferences

To set the dashboard preferences:

1. Click the wrench icon at the top right of the dashboard.
1. Edit the preferences.
   -   Dashboard name and description
   -   Whether to display:
       - Description
       - [Section link bar](#sections)
       - [Dashboard variables](dashboards_variables.html)
   -   A global [events() query](events_queries.html) controlled by the [Show Events](charts_events_displaying.html#controlling-events-overlays) dropdown.
   -   Chart title display properties
   -   Default time window. 
1. Click **Save**.

## Cloning a Dashboard

1. To clone a dashboard:
  -   From the All Dashboards page, locate the dashboard, click the three dots, and select Clone.
  ![clone dashboard](images/dashboard_clone.png)
  -   From a dashboard, click the pencil icon on the right side below the task bar, and select **Clone**.
1. Specify a new URL.
1. Optionally edit the dashboard name.
1. Click **Clone**.

## Editing a Dashboard

{% include shared/system_dashboard.html %}

When editing a dashboard, you can make changes to the dashboard description, name, [dashboard variables](dashboards_variables.html), sections, and charts.

1. To edit a dashboard:
-   From the All Dashboards page, locate the dashboard, click the three dots on the left and and select **Edit**.
![edit dashboard](images/dashboard_edit.png)
-   From a dashboard, click the pencil icon on the right side below the task bar, and select **Edit**.

   The dashboard displays in edit mode.
1. Edit the dashboard.

   When editing a dashboard, you may make several changes at a time. To remove a change, click the revert icon to the left of **Edit JSON** on the task bar. The revert icon removes changes starting with the most recent and works backwards. You can remove only changes made in the current edit mode session.
2. To save configuration changes, click the  **Save** button in the top right.

   ![save dashboard](images/save_dashboard.png)

### Setting the Dashboard Name and Description

1. Click the name of the dashboard. The [Dashboard Display Preferences](#prefs) dialog displays.
1. Rename your dashboard and enter a description.
1. If you enter a description and want it to be displayed on your dashboard, click **Show Description**.
1. Click **Accept**.

<span id="sections"/>

### Configuring Dashboard Sections

Charts are contained in sections. By default, every dashboard has at least one section. A section link bar displays directly below the time bar at the top of the dashboard.

![Section_Table_of_Contents](images/section_links.png)

Jump to a section by clicking the link in the section bar. If the section link bar is not displayed, turn it on in the [Dashboard Display Preferences](#prefs).

![Section_Editing](images/section_actions.png)

While editing a dashboard, available sections actions are:

- Change the name:
  1. Click the name at the far left.
  1. Type a new name.
  1. Press **Enter** or **Return** or click anywhere away from the section name.
- Move, add, or delete. Click icons on the far right.

### Resizing Rows

Resize a row within a section by clicking the resize icons ![resize.png](images/row_resize.png#inline) on the left side of each row. Resizing a row resizes every chart in that row.

### Editing the Dashboard JSON

You can directly modify any dashboard properties by editing the dashboard's JSON format. Use this option only if you have a good understanding of JSON. To access the JSON editor:

1. Put the dashboard in edit mode.
1. Click **Edit JSON** next to the **Save** button on the task bar. The JSON format displays in the Tree editor. You can use the tree controls to expand, collapse, and reorder nodes.

   You can also edit the JSON code directly by selecting **Tree > Code**. For example, to change a row height, edit the row's `heightFactor` property:

   ```
   ...
   ],
     "heightFactor": 100
   },
   ...
   ```

1. Click **Accept**.

## Deleting Dashboards

To delete one or more dashboards:

  -  In the All Dashboards page:

     - Select the checkboxes next to one or more dashboards and click the trash can.
     - Locate a dashboard, select the three dots to the left, click **Delete**, and confirm the delete action.
  -  From a dashboard, click the pencil icon, select **Delete**, and confirm the delete action.

The dashboard(s) are moved to the Trash. You can view the Trash on the All Dashboards page by clicking the trash toggle.

You can recover a dashboard that was moved to the Trash less than 30 days ago. After 30 days, the deleted dashboard is removed from the Trash and you can no longer recover it. To permanently remove a dashboard before the 30 day deadline, toggle the Trash icon and manually delete it.

## Managing Dashboard Versions

Wavefront keeps track of changes made to a dashboard and displays those changes in the Dashboard History page. You can access a dashboard's history from the All Dashboards page or directly from the dashboard:

-   From the All Dashboards page, find the desired dashboard in the list, click the three dots on the left, and select **Versions**.
-   From a dashboard,  click the pencil icon, and select **Versions**.

On the Past Versions page, each revision is listed along with the user who made the change, the day and time it was updated, and the change description.

To revert a change,  select  **Revert**  next to the desired version to go back that version. When you confirm, you are sent to the dashboard version you chose. If you go back to the Past Versions page, the change is reflected on the list.

If you want to view a previous dashboard version before reverting, click the number associated with the dashboard change on the Past Versions page.  You can clone a specific dashboard version by selecting a revision, and then performing a clone operation.

## Managing Dashboard Tags

See [Organizing with Tags](tags_overview.html).

## Managing Charts in Dashboards

To make changes to a chart in a dashboard, put the dashboard in [edit mode](#editing-a-dashboard). After making a change, click **Save**.


### Deleting, Cloning, and Editing a Chart

1. Click the pen icon to enter edit mode.
1. Hover over a chart. Three icons display in the bottom left corner of the chart box ![three_icons](images/chart_actions.png#inline).
1. Delete, clone, or edit the selected chart. When you clone a chart, it is created in a separate row below the selected chart.
**Note** When you clone a chart, specify just the name, e.g.`myurl` in the URL field. Do not specify the full URL.

You can also edit a chart by clicking the chart name in the top right corner. Save your changes by clicking **Save** in the chart.

### Moving a Chart

To move a chart:

1. Hover over a chart.
1. Left-click your mouse and hold. The cursor changes to <i class="fa-arrows fa"/>.
1. Drag the chart to a new position on the dashboard. You can move the chart to an existing row or to a new row by dragging a chart over an <i class="fa-plus-circle fa"/> Add New Chart box.

   You can place up to four charts in a row. More charts in a row means smaller charts. Smaller charts means fewer point buckets and a larger the amount of time represented by each point bucket. See [Chart Resolution](ui_charts.html#chart-resolution).

1. Release the mouse button to move the chart.
