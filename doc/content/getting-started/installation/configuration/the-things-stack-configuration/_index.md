---
title: "The Things Stack Configuration"
description: ""
weight: 2
---

Configuration options for running {{% tts %}} are specified in the `ttn-lw-stack-docker.yml` file. This section points out the required configuration options.

<!--more-->

The example `ttn-lw-stack-docker.yml` file for {{% tts %}} Enterprise shown below contains details which help you follow this section.

<details><summary>Example ttn-lw-stack-docker.yml file</summary>
{{< highlight yaml "linenos=table" >}}
{{< readfile path="/content/getting-started/installation/configuration/ttn-lw-stack-docker-enterprise.yml" >}}
{{< /highlight >}}</details>

### License {{< distributions-inline "Enterprise" >}}

{{% tts %}} Enterprise requires a license, which can be purchased at the [products page](https://thethingsindustries.com/technology/pricing). This is specified in the `license` field, and can be either a `key` string, or a `file`path. See the [License configuration reference]({{< ref "/reference/configuration/the-things-stack#license" >}}) for more information.

### TLS

This example shows the configuration for using TLS with Let's Encrypt. Since {{% tts %}} is being deployed on
`thethings.example.com` in this guide, it is configured to only request certificates for that
host, and also to use it as the default host. See the [TLS Options configuration reference]({{< ref "/reference/configuration/the-things-stack#tls-options" >}}) for more information.

>**Note:** Make sure that you use the correct `tls` configuration depending on whether you are using Let's Encrypt or your own certificate files.

### HTTP

In the `http` section, HTTP server keys for encrypting and verifying cookies are configured, as well
as passwords for endpoints that you may want to keep for the internal use.

### Email

{{% tts %}} sends emails to users, so the `email` section of the configuration defines how these are sent.
You can use Sendgrid or an SMTP server. If you skip setting up an email provider,
{{% tts %}} will print emails to the stack logs.

### Component URLs

Finally, the `console` section configures the URLs for the Web UI and the secret used
by the console client. These tell {{% tts %}} where all its components are accessible.

>**Note:** Failure to correctly configure component URLs is a common problem that will prevent the stack from starting. Be sure to replace all instances of `thethings.example.com` with your domain name! If using `localhost`, see [Special Considerations - Localhost]({{< ref "getting-started/installation/configuration/localhost" >}}) section.

>**Note:** Note that the `client-secret` is also needed when initializing {{% tts %}}.
