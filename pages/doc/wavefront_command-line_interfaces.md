---
title: Wavefront CLIs
keywords: getting started
tags: [getting started]
sidebar: doc_sidebar
permalink: wavefront_clis.html
summary: Learn about the different command-line interfaces you can use with Wavefront.
---

## CLI from Swagger

You can use the Wavefront REST API for general management tasks is publicly available via Swagger. You can use Swagger to generate an [API client](wavefront_api.html#generate-an-api-client-using-swagger) that includes CLI options.


## External CLIs for the REST API

Several Wavefront customers have generated CLIs from our REST API and made them available on Github.

{% include note.html content="These external CLIs are not supported or maintained by Wavefront." %}

* Robert Fisher of [Sysdef Ltd.](https://sysdef.xyz.com) created a [Ruby CLI](https://github.com/snltd/wavefront-cli) and gives installation instructions, examples, and more in [this post](https://sysdef.xyz/post/2017-07-26-wavefront-cli)
  We show how to use it in our blog post [Commanding the Waves Using the Wavefront CLI](https://www.wavefront.com/commanding-the-waves-using-wavefront-cli/)
* Wavefront customer Box open-sourced `wavectl`, their [automation tooling for Wavefront](https://github.com/box/wavectl) in June 2018. This command-line line client for Wavefront is inspired by kubectl and git command line tools. A [Wavefront blog post](https://www.wavefront.com/automation-tooling-wavectl/) discusses that CLI and includes links to the extensive doc and examples.
