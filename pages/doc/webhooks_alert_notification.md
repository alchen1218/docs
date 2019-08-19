---
title: Create and Manage Custom Alert Targets
keywords: alert targets
tags: [alerts, integrations]
sidebar: doc_sidebar
permalink: webhooks_alert_notification.html
summary: Create custom alert targets to receive alert notifications on different messaging platforms.
---

You can create custom alert targets to configure alert notifications for a variety of messaging platforms (email, pager services) and communication channels. You can route notifications for the same alert to different targets based on a point tag.


<div markdown="span" class="alert alert-info" role="alert"><strong>Note</strong> While every Wavefront user can view alert targets, you must have [Alert Management permission](permissions_overview.html) to create and manage alert targets. If you do not have permission, the UI menu selections, buttons, and links you use to perform management tasks are not visible.</div>


## Learn About Alert Targets

This page explains how to create and manage a custom alert target.

* You can further [customize the contents](alert_target_customizing.html) of the alert notifications using Moustache syntax.

* Our blog post [Engineering Tips Series: How Wavefront's Devops Team Uses Alert Targets to Provide Exceptional Quality of Services to Customers](https://www.wavefront.com/engineering-tips-series-wavefronts-devops-team-uses-alert-targets-provide-exceptional-quality-services-customers/) explains how alert targets help Wavefront to keep things running smoothly.

* For the following integrations, you can follow the steps in the integration. Log in to your Wavefront instance or look at the following pages:
  - [PagerDuty Integration](pagerduty.html)
  - [VictorOps Integration](victorops.html)
  - [Slack Integration](slack.html)
  - [HipChat Integration](hipchat.html)


## Why Alert Targets?

Alert targets specify when and how to send notifications.

During alert creation, you can specify an email address or a PagerDuty key in the target list. You implicitly use  built-in Wavefront alert targets. These simple alert targets:

* Cause notifications to be sent whenever the alert is firing, updated, resolved, snoozed, or in a maintenance window.
* Provide internal templates for the notification contents. These internal templates are maintained by Wavefront, and may change from release to release.

For more flexibility, you can create custom alert targets that specify.
* Where to send notifications
* What kind of information
* Notification format
* Which alert events should trigger notifications.

For example, you could use a custom alert target to:

* Expand (or limit) the set of triggering events for notifications.
* Configure different contents for notifications triggered by different events.
* Associate a short name with a long list of email addresses or a lengthy PagerDuty key.

## View Custom Alert Targets

To view alert targets, select **Browse > Alert Targets**.

## Create a Custom Alert Target

The process for creating an alert target is similar for the different types of targets. Setting the **Type** changes which fields are displayed.

1.  Select **Browse > Alert Targets**.
1.  Click the **Create Alert Target** button.
1.  Fill in the properties that are common to all alert targets.
    <table>
    <tbody>
    <thead>
    <tr><th>Property</th><th colspan="2">Description</th></tr>
    </thead>
    <tr>
    <td><strong>Name</strong></td>
    <td colspan="2">Name of the alert target. Pick a name that is simple and that makes it easy to identify the alert target's purpose.</td>
    </tr>
    <tr>
      <td><strong>Description</strong></td>
      <td colspan="2">Description of the alert target. </td>
    </tr>
    <tr>
    <td><strong>Triggers</strong></td>
    <td colspan="2">One or more <a href="alerts_states_lifecycle.html">alert state changes</a> that trigger the alert target. The options are:
    <ul>
    <li><strong>Alert Firing</strong> - Trigger when the alert transitions from checking to firing.</li>
    <li><strong>Alert Status Updated</strong> - Trigger when at least one time series changes category while the alert continues firing. For example, an individual time series could start to fail (satisfy the alert condition during the <strong>Alert fires</strong> time window) or could recover (stop satisfying the alert condition during the <strong>Alert resolves</strong> time window).</li>
    <li><strong>Alert Resolved</strong> - Trigger when the alert resolves.</li>
    <li><strong>Alert Affected by Maintenance Window</strong> - Trigger when a firing alert is affected by a maintenance window.</li>
    <li><strong>Alert Snoozed</strong> - Trigger when the alert is snoozed.</li>
    <li><strong>Alert Has No Data</strong> - Trigger when the time series associated with the alert have all stopped reporting data.</li>
    <li><strong>Alert Has No Data Resolved</strong> - Trigger when at least one time series associated with the alert has started reporting data, while all other time series are still reporting no data.</li>
    <li><strong>Alert Entered Maintenance From No Data</strong> - Trigger when none of the alert's time series are reporting data, and the alert is affected by a maintenance window.</li>
    </ul>
    </td></tr>
    </tbody>
    </table>

1.  From the **Type** pull-down menu, select the alert target type:
    <table>
    <tbody>
    <thead>
    <tr><th>Type</th><th colspan="2">Description</th></tr>
    </thead>
    <tr>
    <td><strong>Webhook</strong></td>
    <td colspan="2">Alert target for sending notifications to messaging platforms such as Slack, VictorOps, or HipChat. This alert target defines the HTTP callback (POST request and URL) that is triggered when an alert changes state. </td>
    </tr>
    <tr>
    <td><strong>Email</strong></td>
    <td colspan="2">Alert target for sending notifications to email systems. This alert target specifies the attributes of the email messages to be sent when an alert changes state.</td>
    </tr>
    <tr>
    <td><strong>PagerDuty</strong></td>
    <td colspan="2">Alert target for sending notifications to PagerDuty. This alert target specifies the PagerDuty key and a POST body to use when an alert changes state.</td>
    </tr>
    </tbody>
    </table>

1.  Fill in the type-specific properties.
* For type **Webhook**, set these properties:
      <table>
      <tbody>
      <thead>
      <tr><th>Webhook Property</th><th>Description</th></tr>
      </thead>
      <tr>
      <td><strong>URL</strong> </td>
      <td markdown="span">REST endpoint of the messaging platform to receive the alert notification. You can follow the setup steps in [Slack Integration](slack.html), [VictorOps Integration](victorops.html), or [HipChat Integration](hipchat.html) to obtain a notification URL. The notification URL must be publicly accessible. </td>
      </tr>
      <tr>
        <td><strong>Content Type</strong></td>
        <td>Content type of the POST body:
          <ul>
            <li>application/json</li>
            <li>text/html</li>
            <li>text/plain</li>
            <li>application/x-www-form-urlencoded</li>
        </ul></td>
      </tr>
      <tr>
        <td><strong>Custom Headers</strong> </td>
        <td>Name and value of one or more HTTP headers to pass in the POST request.</td>
      </tr>
      <tr>
        <td><strong>Body Template</strong> </td>
        <td markdown="span">Template describing the contents of the alert notification. Click **Template** and select the template that corresponds your messaging platform: **Slack**, **VictorOps**, or **HipChat**. Or, select **Generic Webhook** to see all of the available content options combined in a single template.</td>
      </tr>
      </tbody>
      </table>
* For type **Email**, set these properties:
      <table>
      <tbody>
      <thead>
      <tr><th>Email Property</th><th>Description</th></tr>
      </thead>
      <tr>
        <td><strong>HTML Format</strong> </td>
        <td>Specifies whether the email platform should interpret the message body as HTML or plain text. When checked (the default), messages are interpreted as HTML. It is your responsibility to coordinate this setting with the chosen <strong>Body Template</strong> option.  </td>
      </tr>
      <tr>
        <td><strong>Email Address List</strong> </td>
        <td>One or more valid email addresses, separated by commas. </td>
      </tr>
      <tr>
        <td><strong>Email Subject</strong> </td>
        <td>Subject of all emails from this alert target. </td>
      </tr>
      <tr>
        <td><strong>Body Template</strong> </td>
        <td markdown="span">Template describing the contents of the alert notification. Click **Template** and select the template that corresponds to your email formatting preference: **HTML Email** or **Plain Text**. It is your responsibility to coordinate this option with the **HTML Format** setting.</td>
      </tr>
      </tbody>
      </table>
* For type **PagerDuty**, set these properties:
      <table>
      <tbody>
      <thead>
      <tr><th>PagerDuty Property</th><th>Description</th></tr>
      </thead>
      <tr>
        <td><strong>PagerDuty Key</strong> </td>
        <td markdown="span">API integration key for the PagerDuty application. Follow the setup steps in [PagerDuty Integration](pagerduty.html) to obtain the key.</td>
      </tr>
      <tr>
        <td><strong>Body Template</strong> </td>
        <td markdown="span">Template describing the contents of the alert notification's subject line. Click **Template** and select the **PagerDuty Subject** template.</td>
      </tr>
      </tbody>
      </table>
1. If you want to send notifications to different targets for different point tags, you can specify them under **Recipients**.
  * The **Default Recipients** field specifies recipients that get all alerts.
  * The **Routing** field allows you to specify point tag and point tag value pairs and specify the target for each pair.
  ![alert route example](images/alert_route_example.png)
1. Optionally customize the **Body Template** using the variables and functions described in [Customizing Alert Target Templates](alert_target_customizing.html).
1. Click **Save** to add the alert target and make it visible on the Alert Targets page.
1. [Test](#testing-an-alert-target) your new alert target, and then [add it to an alert](#adding-a-custom-alert-target-to-a-wavefront-alert).


## Test a Custom Alert Target

Test your alert target to ensure that it works properly.

1. Select **Browse > Alert Targets** and find the target on the Alert Targets page.
1. Click the three dots to the left of the alert target and select **Test**.

  ![alert target test](images/alert_target_test.png)

## Add a Custom Alert Target to a Wavefront Alert

To add a custom alert target to a new or existing alert:

1. Display the [Create Alert](alerts.html#creating-an-alert) page or the [Edit Alert](alerts.html#editing-an-alert) page.
1. Scroll down to the **Target List** section.
1. In the **Alert Target** field, start typing. A dropdown list appears that contains all available Wavefront alert targets that can be integrated to your alert.
1. Select the alert target that you want to add, and click **Save**.


## Edit a Custom Alert Target

You can change a custom alert target at any time.

To edit a alert, click the alert target name in the Alert Targets browser or click the three dots to the left of the alert target and select **Edit**.

## Delete Custom Alert Targets

You can delete one or more custom alert targets by checking the checkboxes next to the alert targets and clicking the Trash icon <i class="fa fa-trash"/> at the top of the Alert Targets page. The trash icon is grayed out if you don't have permission to delete any of the selected alert targets.

To delete one alert target, use the trash icon or click the three dots to the left of the alert target and select **Delete**.

## Find an Alert Target ID

Each custom alert target has a unique ID that the system generates when you first create the alert target. To find the ID:

1. Click **Browse > Alert Targets**.
1. In the Name column, note the ID of the alert target under the description.

   ![webhook ID](images/webhook_id.png)


## Query Responses of Webhook Alert Targets

Wavefront exposes response codes from webooks alert target calls as metrics:

```
~alert.webhooks.<webhook_id>.1xx
~alert.webhooks.<webhook_id>.2xx
~alert.webhooks.<webhook_id>.3xx
~alert.webhooks.<webhook_id>.4xx
~alert.webhooks.<webhook_id>.5xx
```

**Note** Wavefront does not expose response codes from the simpler alert targets (Email and PagerDuty).

The response codes indicate if a webhook call was successful and if the webhook generated a notification. You can query these metrics to  determine if any webhooks are generating a problem response code. The metrics have the point tag `name = <webhook_name>` so you can determine all the response codes for a particular webhook alert target:

```
ts(~alert.webhooks.*.*, name=<webhook_name>)
```

If the response code of the webhook is anything other than 2xx, Wavefront creates an event with the name `<webhook_id>.<webhook_name>.<response_code>`.
