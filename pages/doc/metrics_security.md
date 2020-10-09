---
title: Metrics Security Policy Rules
keywords: administration
tags: [administration]
sidebar: doc_sidebar
permalink: metrics_security.html
summary: Use metrics security to control access to time series, histograms, and delta counters.
---

In a large enterprise, certain data are confidential. Wavefront allows you to limit who can see or modify data in several ways.
* **Permissions** are **global** settings.
  - Some permissions limit who can modify Wavefront objects. For example, only users with **Dashboards** permission can modify dashboards.
  -  Other permissions make certain information completely invisible. For example, only users with SAML IDP Admin permission can see the **Self Service SAML** menu or access that page.
* **Access Control** allows administrators with the right permissions fine-grained control over individual dashboards or alerts. For example, it's possible to limit view and modify access to a Finance_2020 dashboard to just the Finance department.
* **Metrics Security** supports even finer-grained control. In the example above, access to the Finance_2020 dashboard is limited to the Finance department. With metrics security, you can limit access to confidential time series, histogram and delta counter metrics to the leadership team.

{% include note.html content="Only a Super Admin user or users with **Metrics** permission can view, create, and manage metrics security policy. " %}

## How Metrics Security Protects Sensitive Data

Metrics security policy rules allows fine-grained support for limiting access to sensitive data.

### Block or Allow Access

With metrics security policy rules you can block or allow access
* To metrics, optionally filtered by source and/or point tag
* Based on groups, roles, and/or individual users.

When an account attempts to access metrics, the backend looks at the rules in priority order. Higher priority rules overwrite lower priority rules.

For example, assume you have two rules:

<table style="width: 100%;">
<tbody>
<thead>
<tr><th width="30%">Name</th><th width="15%">Priority</th><th width="30%">Metrics</th><th width="30%">Accounts</th></tr>
</thead>
<tr>
<td markdown="span">BlockRevenueNumbers</td>
<td>2</td>
<td>All metrics that start with <code>revenue*</code></td>
<td>All accounts</td>
</tr>
<tr>
<td markdown="span">AllowRevenueFinance</td>
<td>1</td>
<td>All metrics that start with <code>revenue*</code></td>
<td>All accounts in Finance group</td>
</tr>
</tbody>
</table>

After the rules are in force, only users in the Finance group can access data that start with `revenue*`.


### Sensitive Data Become Invisible

Data protected by a metrics security policy rule can become invisible to users.

* **Not visible in charts**. The chart either includes a warning that some metrics are protected, or, if all metrics are protected, the chart shows only the message.
* **Not visible in alerts** (if **Secure Metrics Details** is checked for the alert). The alert fires based on the complete set of metrics, and the complete set is shown in notification images by default. A checkbox allows administrators to [hide alert details](alerts_notifications.html#alert-notification-with-secured-metrics-details) so that confidential metric are not shown.
* **Not visible in auto-complete** in Chart Builder, Query Editor, Metrics browser, etc.

### Rule Priority and Rule Pairs

Rules are evaluated in priority order. In many cases, it's useful to think of pairs of rules, for example:

* First block access to all metrics for a group (Priority 2)
* Allow access to a small set of metrics (e.g. `*dev*`) for that group (Priority 1)

Because Priority 1 overrides Priority 2, the group has access to a small set of metrics.

See the Examples below for some scenarios.

### Alert Notifications

To protect metrics from inclusion in alert notifications, use the [**Secure Metrics Details** check box](alerts_notifications.html#alert-notification-with-secured-metrics-details). Wavefront looks at all metrics when determining when an alert status should change and shows them in alert notifications.

### Derived Metrics and Events

The current implementation does not protect metrics in Derived Metrics or in Events.



<!--Include text of warning message-->

<!---

## Example: Limited Access for Engineering

Consider this simple example:

* An environment includes metrics that the developer team needs to see and some financial metrics that only the CFO and finance team should see.
* Your main priority is to protect the sensitive metrics, so you create 2 rules:

<table style="width: 100%;">
<tbody>
<thead>
<tr><th width="30%">Name</th><th width="15%">Priority</th><th width="30%">Metrics</th><th width="30%">Accounts</th></tr>
</thead>
<tr>
<td markdown="span">Block</td>
<td>2</td>
<td>Block all metrics</td>
<td>Developer group</td>
</tr>
<tr>
<td markdown="span">Allow Infra and Related Metrics</td>
<td>1</td>
<td>Metrics with point tag env=production or env=dev (but not env=finance)</td>
<td>All accounts in Developer group.</td>
</tr>
</tbody>
</table>


![two rules, described above, shown in UI](images/m_security_rules.png)
--->


## Create a Metrics Security Policy Rule

When you create a Metrics Security Policy rule, you specify the metrics you want to protect (or make available) and the account, group, or role that should (or should not) have access to those metrics.

{% include note.html content="Only a Super Admin user or users with **Metrics** permission can view, create, and manage metrics security policy. " %}

Here's an annotated screenshot that shows the main actions:

![Annotated Edit Rule screenshot. Highlights Press Enter in Prefix / Source and Point Tag section](images/metrics_s_edit_rule.png)


When you create a rule, plan your strategy.

* **Metrics Dimensions** allow you to determine what to block or allow.
  - Specify one or more metric prefixes. You can specify exact an match (e.g. `requests` or `request.`) or a wildcard match (e.g. `*.cpuloadav*`, `cpu.*`).
  - Specify a combination of metric sources or point tags to narrow down the metrics. For example, you could block visibility into production environments for some developers, or block some development environments metrics for contractors.
* **Access** allows you to allow or block access for a combination of accounts and/or groups or for roles.


1. From the gear icon, select **Metrics Security Policy** and click **Create Rule**
2. In the **Create Rule** dialog, specify the rule parameters.
  1. Specify a descriptive name. Users might later modify the rule, so a clear name is essential.
  2. Add a description. The description is visible only when you edit the rule. The name is visible on the Metrics Security Policy page.
  3. Specify the metrics that you want to protect (or make available) by using a metrics prefix. You can specify the metric (e.g. `~sample.network.bytes.sent`) or a wildcard match (e.g. `~sample.network.bytes.*` or `~sample.network.*`)
  4. Describe the metrics:
     * You can specify the full metric name or use a wildcard character in metric names, sources, or point tags. The wildcard character alone (`*`) means all metrics.
     * Specify key=value pairs, for example, `source="app-24"` or `env=dev`.
     * If you want to specify multiple key=value pairs, select on the right whether you want to combine them with `and` or `or`.
  5. Specify the Access definition for the rule.
     1. Select **Allow** or **Block** from the menu.
     2. Specifying accounts, groups, or roles.
  3. Click **OK.**

## Manage Metrics Security Policy Rules

The following annotated screenshot gives an overview of rule management options:

![screenshot, annotated with the items explained below](images/metrics_security_annotated.png)

<!---Have to change screenshot to show Save instead of Apply--->

* In the top row, you can click **Create Rule**. Each newly newly created rule has Priority 1 by default, which means it overrides all other rules. Drag and drop rules to change priority.
* The **Version History** button allows you to revert to an earlier version of the policy.
* Information on who last edited the security policy and when that happened is always included.
* Select one or more rules by clicking the check box, then use the icons above to clone or delete selected rules. You can change several rules, and then click **Save** to commit the changes.
* You can also hover over the six-dot icon to explicitly drag a rule where you want it.
* If you've moved, cloned, or deleted one or more rules, use the **Undo** button to undo the change, or **Redo** to revert the undo.
* The **Metric Prefix** column shows the metrics affected by a rule.
* The **Access** column shows whether the rule allows or blocks access.


## Example for Metrics Security Policies

Before you start, plan your strategy. Here are some common scenarios.

Initially, a single metrics security policy rule is defined:

![screenshot of default single policy](images/metrics_security_default.png)

All users can access all metrics, meaning **no** restrictions are in place.  Furthermore, if the single **Allow All Metrics** rule was deleted, all users would still have access to all metrics.

### Example: Restrict Access to Confidential Metrics

This example restricts access to specific ranges of highly-sensitive metrics, say revenue numbers, to select groups of users.

![screenshot of default single policy](images/metrics_security_restrict.png)

The image above shows how to restrict metrics starting with `revenue.*` to  be accessible only by members of the group `Finance`. The policy grants all users access to all other metrics.

* When the metric `revenue.saas` is queried by a user in the `Finance` group, this access matches Rule 1 (**Finance Group can access Revenue**).  The rule grants the access, so the metric is shown to the user and no other rules are consulted.

* When the metric `revenue.saas` is queried by a user **not** in the `Finance` group, the access does **not** match Rule 1.  The engine moves on to Rule 2 (**No one else can access Revenue**), which matches because all users belong to the Everyone group.  Because the rule denies the access, the metric is not shown to the user. No other rules are consulted.

### Example: Restrict Access for a Group of Users

This example restricts access for a group of users, making only a subset of the metrics in the system available to them.

![screenshot of default single policy](images/metrics_security_group.png)

The image above shows how to restricts access for users in the group `Contractors`. Those users can only query metrics tagged with the point tag `env=dev`. This policy imposes no restrictions on any other groups.

* When a user belonging to group `Contractors` runs a query for `cpu.usage` tagged with `env=dev`, this access matches Rule 1 (**Contractors can access dev environment metrics**) and access is granted.
* But when the user issues a query for `cpu.usage` tagged with `env=prod`, this access does not match Rule 1. Rule 2 (**Contractors cannot access any other metrics**) acts as a catch-all for users of group `Contractors` and denies them access to this metric.

### Example: Strictly Limit Access on a Need-to-Know Basis

Some companies want to make any metric acceessible only to the team that needs to know about it. Access to metrics outside a team’s scope of work is disabled.  Only administrators are allowed access to all metrics.

![screenshot of default single policy](images/metrics_security_need_to_know.png)

The image above shows how to use use a set of rules to accomplish this.

* Rule 4 (**Block All Metrics by default**) applies to any access that doesn't match a higher-up rule. It denies access to all users. Users get access only when an "exception" rule with higher priority access matches.
* Rule 3 (**Allow All Metrics to Admins**) grants access to all metrics to users in the `Admins` group.
* Rule 2 (**Allow Gadgets Team access to Gadget Metrics**) grants access to any metrics starting with `gadget.*` to members of the `Gadgets` group.
* Rule 1 (**Allow Widgets Team access to Widget Metrics**) grants access to any metrics starting with `widget.*` to members of the `Widgets` group.

In this example, ordering (priority) between rules 1 and 2 does not matter because describe rules for independent metric regions.

With this policy in place:
* Members of the `Widgets` group are granted access if the metric starts with `widget.*` (Rule 1) and denied otherwise (Rule 4).
* Members of the `Gadgets` group are granted access if the metric starts with `gadget.*` (Rule 2) and denied otherwise (Rule 4).
*  Members of the `Admins` group are granted access to all metrics (Rule 3).

Any user not belonging to one of the groups covered by the rules have no access.
