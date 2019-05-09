---
title: Searching Wavefront
keywords: getting started
tags: [getting started, video]
sidebar: doc_sidebar
permalink: wavefront_searching.html
summary: Learn how to search for objects in the Wavefront UI.
---
To help you find exactly what you need, Wavefront supports tags and a variety of search features.

Here's a video to get your started:

<p><a href="https://vmwarelearningzone.vmware.com/oltpublish/site/openlearn.do?dispatch=previewLesson&id=5468d6de-dc7a-11e7-a6ac-0cc47a352510&inner=true&player2=true"><img src="/images/v_searching.png" style="width: 700px;"/></a>
</p>

## Using Tags to Facilitate Searches

Tags allow you to flexibly manage and organize your Wavefront content.
* Tag paths allow you to organize your content in hierarchies that best suit your particular use of Wavefront.
* You can include content in multiple hierarchies to suit the needs of different groups of users.

We support several different types of tags that you can leverage when searching. See [Organizing with Tags](tags_overview.html)

## Searching

All Wavefront browsers (**All Dashboards**, **Alerts**, **Integrations**, and so on) support a variety of search methods. You can search in the Search field at the top or in the faceted filter bar at the left.

### Search Field

The search field at the top of every Wavefront browser page supports both autocomplete and search. We support autocomplete for many searches.

For example, in the All Dashboards browser page, searching returns a dropdown list that displays:

* autocompleted sources and metrics queried in dashboards
* dashboard names
* tags containing the search string

You can select each item in the list individually. The dropdown list also contains links to searches for _all_ tags and dashboard content containing the string. For example:

![search auto](images/search_auto.png)

Search fields support multi-word searches. If you type **cpu usage** in any browser or autocompleted text field, the dropdown list includes all matches for both **cpu** and **usage**.

### Filter Bar

In the filter bar on the left you search by selecting facets, such as **State** and **Severity** for alerts, and by typing in Search fields. Some facets one some pages have their own Search field to limit the displayed facet values. Most pages support the standard facets Tag Paths, Tags, and Updated By.

### Saved Searches

Most filter bars contain a set of commonly used saved searches (e.g. Favorites, Last Updated, Recently Updated, My <XXX>) and you can save your own searches.

Once you press **Return** or **Enter** after typing a search string, the icons ![search icons](images/searchicons.png#inline) display at the top right, allowing you to share a link to, save, and clear the search. Your saved searches appear below the commonly used searches, and have a dropdown menu for renaming, cloning, and deleting the search.

The following Alerts browser filter demonstrates filtering alerts by the tag path **Microservice.App4**. This filters the view to show all alerts with the tag path **MicroService.App4** and all its children (for example, **MicroService.App4.Auth**). Of the matching alerts, 1 is firing.

![Tag path](images/MicroService.App4_firing.png)


## Other Actions

The Wavefront UI supports tagging and other actions.

-   **Tagging** - Make a selection and click the tag buttons to create, add, and remove tags.
-   **Actions** - Perform actions (clone, delete, edit, rename, etc.) on an individual object (e.g. an alert) by clicking the three dots to the far left of the object, for example, to the left of an alert. Actions are different for different objects but might include Clone, Delete, or Test (for alerts). Here's an example for the Actions menu for dashboards:

    ![dashboard clone](/images/dashboard_clone.png)

-   **Trash** - Click the trash toggle to view deleted objects.
