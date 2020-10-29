---
title: Okta Integration
tags: [integrations list]
permalink: okta.html
summary: Learn about the Wavefront Okta Integration.
---
## Okta Integration

Okta is a popular identity management product that can be integrated with Wavefront to enable single sign-on.
## Okta Setup

### Step 1. Create the Okta Application

1. In Okta, click **Add Applications**. 
1. Click **Create New App**. 
1. In the Create a New Application dialog, select **SAML 2.0** and click **Create**.
1. In the **App name** field, type **Wavefront**.
1. Right-click and save the Wavefront logo: 
   {% include image.md src="images/wavefront_logo_okta.png" width="25" %}
1. In the **App logo** field, browse to the logo file and click **Upload Logo**.
1. Click **Next**. 
1. Enter the following SAML settings:
    - **Single sign on URL** - https://YOUR_CLUSTER.wavefront.com/api/saml/login
    - **Use this for Recipient URL and Destination URL** - Checked
    - **Audience URI (SP Entity ID)** - https://YOUR_CLUSTER.wavefront.com
    - **Default RelayState** - &lt;LEAVE BLANK&gt;
    - **Name ID Format** - EmailAddress
    - **Application username** - Email
1. Click **Next**.
1. In the **Are you customer or partner?** field, select **I'm an Okta customer adding an internal app**.
1. In the **App type** field, select **This is an internal app that we have created**.
1. Click **Finish**.
1. In the application Sign On tab, right-click the **Identity Provider metadata** link and select **Copy Link**.

   {% include image.md src="images/okta_metadata_url.png" width="50" %}


### Step 2. Send Identity Provider Metadata to Wavefront

1. Send the link to [support@wavefront.com](mailto:support@wavefront.com) with a request to set up Okta integration for Wavefront. We'll notify you as soon as we've done this. At that point the users would authenticate to Wavefront through Okta instead of using a password. Any new user that comes along that did not yet exist in Wavefront would just get auto-created on the Wavefront side on first authentication.


