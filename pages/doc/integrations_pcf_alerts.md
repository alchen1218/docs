---
title: Pivotal Cloud Foundry Alerts
keywords:
tags: [integrations, proxies]
sidebar: doc_sidebar
permalink: integrations_pcf_alerts.html
summary: Details for Pivotal Cloud Foundry Alerts
---

The Pivotal Cloud Foundry (PCF) integration includes a a rich set of alerts out of the box. You can preview the alerts in the **Alerts** tab of the integration. This page gives details for each alert.

## PAS Active Locks Alerts

Total count of how many locks the system components are holding.

If the ActiveLocks count is not equal to the expected value, there is likely a problem with Diego.
1. Run monit status to inspect for failing processes.
2. If there are no failing processes, then review the logs for the components using the Locket service: BBS, Auctioneer, TPS Watcher, Routing API, and Clock Global (Cloud Controller clock). Look for indications that only one of each component is active at a time.
3. Focus triage on the BBS first:
   - A healthy BBS shows obvious activity around starting or claiming LRPs.
   - An unhealthy BBS leads to the Auctioneer showing minimal or no activity. The BBS sends work to the Auctioneer.
   - Reference the BBS-level Locket metric bbs.LockHeld. A value of 0 indicates Locket issues at the BBS level. For more information, see Locks Held by BBS.
4. If the BBS appears healthy, then check the Auctioneer to ensure it is processing auction payloads.
   - Recent logs for Auctioneer should show all but one of its instances are currently waiting on locks, and the active Auctioneer should show a record of when it last attempted to execute work. This attempt should correspond to app development activity, such as `cf push`.
   - Reference the Auctioneer-level Locket metric `auctioneer.LockHeld`. A value of 0 indicates Locket issues at the Auctioneer level. For more information, see Locks Held by Auctioneer.
5. The TPS Watcher is primarily active when app instances crash. Therefore, if the TPS Watcher is suspected, review the most recent logs.
6. If you are unable to resolve on-going excessive active locks, pull logs from the Diego BBS and Auctioneer VMs, which includes the Locket service component logs, and contact Pivotal Support.

## PAS Auctioneer LRP Auctions Failed

The number of Long Running Process (LRP) instances that the Auctioneer failed to place on Diego Cells.

This metric can indicate that PAS is out of container space or that there is a lack of resources within your environment. This indicator also increases when the LRP is requesting an isolation segment, volume drivers, or a stack that is unavailable, either not deployed or lacking sufficient resources to accept the work. This metric is emitted on event, and therefore gaps in receipt of this metric can be normal during periods of no app instances being scheduled. This error is most common due to capacity issues. For example, if Diego Cells do not have enough resources, or if Diego Cells are going back and forth between a healthy and unhealthy state.
1. To best determine the root cause, examine the Auctioneer logs. Depending on the specific error and resource constraint, you may also find a failure reason in the Cloud Controller (CC) API.
2. Investigate the health of your Diego Cells to determine if they are the resource type causing the problem.
3. Consider scaling additional Diego Cells using Ops Manager.
4. If scaling Diego Cells does not solve the problem, pull Diego Brain logs and BBS node logs and contact Pivotal Support telling them that LRP auctions are failing.

## PAS Auctioneer Task Auctions Failed

The number of Tasks that the Auctioneer failed to place on Diego Cells.

This metric is cumulative over the lifetime of the Auctioneer job. Failing Task auctions indicate a lack of resources within your environment and that you likely need to scale. This indicator increases when the Task is requesting an isolation segment, volume drivers, or a stack that is unavailable, either not deployed or lacking sufficient resources to accept the work. This metric is emitted on event, and therefore gaps in receipt of this metric can be normal during periods of no tasks being scheduled. This error is most common due to capacity issues. For example, if Diego Cells do not have enough resources, or if Diego Cells are going back and forth between a healthy and unhealthy state.
1. In order to best determine the root cause, examine the Auctioneer logs. Depending on the specific error or resource constraint, you may also find a failure reason in the CC API.
2. Investigate the health of Diego Cells.
3. Consider scaling additional Diego Cells using Ops Manager.
4. If scaling Diego Cells does not solve the problem, pull Diego Brain logs and BBS logs for troubleshooting and contact Pivotal Support for additional troubleshooting. Inform Pivotal Support that Task auctions are failing.

## PAS Auctioneer Time to Fetch Diego Cell State

Time in ns that the Auctioneer took to fetch state from all the Diego Cells when running its auction. Indicates how the Diego Cells themselves are performing. Alerting on this metric helps alert that app staging requests to Diego may be failing.
1. Check the health of the Diego Cells by reviewing the logs and looking for errors.
2. Review IaaS console metrics.
3. Inspect the Auctioneer logs to determine if one or more Diego Cells is taking significantly longer to fetch state than other Diego Cells. Relevant log lines have wording like `fetched Diego Cell state`. Pull Diego Brain logs, Diego Cell logs, and Auctioneer logs and contact Pivotal Support telling them that fetching Diego Cell states is taking too long.

## PAS BBS Crashed App Instances

Total number of LRP instances that have crashed. Indicates how many instances in the deployment are in a crashed state. An increase in `bbs.CrashedActualLRPs` can indicate several problems, from a bad app with many instances associated, to a platform issue that is resulting in app crashes. this metric to help create a baseline for your deployment. After you have a baseline, you can create a deployment-specific alert to notify of a spike in crashes above the trend line. Tune alert values to your deployment. Frequency: 30 s
1. Look at the BBS logs for apps that are crashing and at the Diego Cell logs to see if the problem is with the apps themselves, rather than a platform issue.
2. Before contacting Pivotal Support, pull the BBS logs and, if particular apps are the problem, pull the logs from their Diego Cells too.

## PAS BBS Fewer App Instances Than Expected

Total number of LRP instances that are desired but have no record in the BBS. When Diego wants to add more apps, the BBS sends a request to the Auctioneer to spin up additional LRPs. `LRPsMissing` is the total number of LRP instances that are desired but have no BBS record. If Diego has less LRP running than expected, there may be problems with the BBS. An app push with many instances can temporarily spike this metric. However, a sustained spike in bbs.`LRPsMissing` is unusual and should be investigated. Frequency: 30 s

1. Review the BBS logs for proper operation or errors, looking for detailed error messages.
2. If the condition persists, pull the BBS logs and contact Pivotal Support.

## PAS BBS Master Elected

Indicates when there is a BBS master election. A BBS master election takes place when a BBS instance has taken over as the active instance. A value of 1 is emitted when the election takes place. This metric emits when a redeployment of the BBS occurs. If this metric is emitted frequently outside of a deployment, this may be a signal of underlying problems that should be investigated. If the active BBS is continually changing, this can cause app push downtime.

## PAS BBS More App Instances Than Expected

Total number of LRP instances that are no longer desired but still have a BBS record. When Diego wants to add more apps, the BBS sends a request to the Auctioneer to spin up additional LRPs. `LRPsExtra` is the total number of LRP instances that are no longer desired but still have a BBS record. If Diego has more LRPs running than expected, there may be problems with the BBS. Deleting an app with many instances can temporarily spike this metric. However, a sustained spike in `bbs.LRPsExtra` is unusual and should be investigated. Frequency: 30 s
1. Review the BBS logs for proper operation or errors, looking for detailed error messages.
2. If the condition persists, pull the BBS logs and contact Pivotal Support.

## PAS BBS Running App Instances Rate of Change

`DYNAMIC ALERT: NEGATIVE 10` is a placeholder. Rate of change in the average number of app instances being started or stopped on the platform. It is derived from `bbs.LRPsRunning` and represents the total number of LRP instances that are running on Diego cells.

This Delta reflects a DOWNWARD trend for app instances started or stopped. It helps to provide a picture of the overall (lack of) growth trend of the environment for capacity planning. You may want to alert on delta values outside of an expected range.

## PAS BBS Time to Handle Requests

The maximum observed latency time over the past 60 seconds that the BBS took to handle requests across all its API endpoints. If this metric rises, the PAS API is slowing. Response to certain cf CLI commands is slow if request latency is high.
1. Check CPU and memory statistics in Ops Manager.
2. Check BBS logs for faults and errors that can indicate issues with BBS.
3. Try scaling the BBS VM resources up. For example, add more CPUs and memory depending on its `system.cpu`/`system.memory` metrics.
4. Consider vertically scaling the PAS backing database, if `system.cpu` and `system.memory` metrics for the database instances are high.
5. If the above steps do not solve the issue, collect a sample of the Diego Cell logs from the BBS VMs and contact Pivotal Support to troubleshoot further.

## PAS BBS Time to Run LRP Convergence

Time that the BBS took to run its LRP convergence pass. If the convergence run begins taking too long, apps or Tasks may be crashing without restarting. This symptom can also indicate loss of connectivity to the BBS database.
1. Check BBS logs for errors.
2. Try vertically scaling the BBS VM resources up. For example, add more CPUs or memory depending on its `system.cpu`/`system.memory` metrics.
3. Consider vertically scaling the PAS backing database, if `system.cpu` and `system.memory` metrics for the database instances are high.
4. If that does not solve the issue, pull the BBS logs and contact Pivotal Support for additional troubleshooting.

## PAS Cloud Controller and Diego Not in Syncpreview

Indicates if the `cf-apps` Domain is up-to-date, meaning that PAS app requests from Cloud Controller are synchronized to bbs.LRPsDesired (Diego-desired AIs) for execution.
* 1 means cf-apps Domain is up-to-date
* No data received means cf-apps Domain is not up-to-date: If the cf-apps Domain does not stay up-to-date, changes requested in the Cloud Controller are not guaranteed to propagate throughout the system. If the Cloud Controller and Diego are out of sync, then apps running could vary from those desired.
  - 1. Check the BBS and Clock Global (Cloud Controller clock) logs.
  - 2. If the problem continues, pull the BBS logs and Clock Global (Cloud Controller clock) logs and contact Pivotal Support to say that the `cf-apps` domain is not being kept fresh.

## PAS Diego Cell Route Emitter Sync Duration

Time the active Route Emitter took to perform its synchronization pass. Increases in this metric indicate that the Route Emitter may have trouble maintaining an accurate routing table to broadcast to the GoRouters. Tune your alerting values to your deployment based on historical data and adjust based on observations over time. The suggested starting point is ≥ 5 for the yellow threshold and ≥ 10 for the critical threshold. Above 10 seconds, the BBS may be failing.
* Yellow warning: > 5s
* Red critical: >10 s

ACTION:
If all or many jobs showing as impacted, there is likely an issue with Diego. * Investigate the Route Emitter and Diego BBS logs for errors.
* Verify that app routes are functional by making a request to an app, pushing an app and pinging it, or if applicable, checking that your smoke tests have passed. If one or a few jobs showing as impacted, there is likely a connectivity issue and the impacted job should be investigated further.

## PAS Garden Health Check Failed
The Diego Cell periodically checks its health against the Garden back end. For Diego Cells, 0 means healthy, and 1 means unhealthy. Set an alert for further investigation if multiple unhealthy Diego Cells are detected in the given time window. If one Diego Cell is impacted, it does not participate in auctions, but end-user impact is usually low. If multiple Diego Cells are impacted, this can indicate a larger problem with Diego, and should be considered a more critical investigation need.
1. Investigate Diego Cell servers for faults and errors.
2. If a particular Diego Cell or Diego Cells appear problematic:
  1. Determine a time interval during which the metrics from the Diego Cell changed from healthy to unhealthy.
  2. Pull the logs that the Diego Cell generated over that interval. The Diego Cell ID is the same as the BOSH instance ID.
  3. Pull the BBS logs over that same time interval.
  4. Contact Pivotal Support.
3. As a last resort, if you cannot wait for Pivotal Support, it sometimes helps to recreate the Diego Cell by running bosh recreate. For information about the bosh recreate command syntax, see Deployments in Commands in the BOSH documentation. Warning: Recreating a Diego Cell destroys its logs. To enable a root cause analysis of the Diego Cell’s problem, save out its logs before running `bosh recreate`.