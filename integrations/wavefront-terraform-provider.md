---
title: Terraform Provider Integration
tags: [integrations list]
permalink: wavefront-terraform-provider.html
summary: Learn about the Wavefront Terraform Provider Integration.
---
# Wavefront Terraform Provider

The [Wavefront Terraform Provider](https://github.com/spaceapegames/terraform-provider-wavefront) is a custom
terraform provider from [spaceapegames](http://www.spaceapegames.com/) that enables users to manage resources
on Wavefront.  This [spaceapegames blog post](https://tech.spaceapegames.com/2017/09/28/building-a-custom-terraform-provider-for-wavefront/)
explains how and why spaceapegames created the terraform provider.

This integration currently supports alerts, alert targets and dashboards.

## Wavefront Terraform Provider Setup

This setup is an reiteration of [terraform-provider-wavefront](https://github.com/spaceapegames/terraform-provider-wavefront).



### Step 1.  Download and Customize the Plugin

You can download the plugin from this [git repository](https://github.com/spaceapegames/terraform-provider-wavefront/releases).
Latest releases include darwin and linux amd64 packages. 
If you need a different architecture or OS, you can build the plugin from source.
Go to [terraform-provider-wavefront](https://github.com/spaceapegames/terraform-provider-wavefront) for details.

Once you have the plugin, remove the _os_arch from the end of the file name and place it in ~/.terraform.d/plugins which is where terraform init looks for plugins.
See [provider configuration](https://www.terraform.io/docs/configuration/providers.html#third-party-plugins) for syntax details.

### Step 2.  Create a Terraform Config File

Create a main.tf with the following configurations.

{% raw %}
```
provider "wavefront" {
  address = "https://YOUR_CLUSTER.wavefront.com"
  token = "YOUR_API_TOKEN"
}

resource "wavefront_alert" "test_alert" {
  name = "Terraform Test Alert"
  target = "test@example.com"
  condition = "100 - ts(\"cpu.usage_idle\") > 80"
  display_expression = "100-ts(\"cpu.usage_idle\")"
  minutes = 5
  resolve_after_minutes = 5
  severity = "WARN"
  tags = [
    "terraform"
  ]
}
```
{% endraw %}

- In the `provider` block, provide your wavefront address and api token from your account.
  - You can also export the address (WAVEFRONT_ADDRESS) and token (WAVEFRONT_TOKEN) as environment variables
to avoid committing them to source control (We highly recommend you to do this for the token!).
- In the resource block, each field corresponds to an alert object field. You use those alert object fields
when creating alerts with the Wavefront API.

### Step 3.  Running the plugin

- Check that your wavefront account has permission `Alert Management`.

- Run `terraform init` to load your provider.

- Run `terraform plan` to show the plan.
 
- Run `terraform apply` to apply the test configuration and then check the results in Wavefront.
 
- Update main.tf to change a value, the run plan and apply again to check that updates work.

- Run `terraform destroy` to test deleting resources.

