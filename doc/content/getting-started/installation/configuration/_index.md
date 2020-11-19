---
title: "Configuration"
description: ""
weight: 2
---

{{% tts %}} can be configured using command-line flags, environment variables, or configuration files. See the [Configuration Reference]({{< ref "reference/configuration" >}}) for more information about the configuration options.

This guide shows an example of configuring {{% tts %}} using a configuration file, with an example domain `thethings.example.com` and TLS certificates from Let's Encrypt.

>**Note:** If configuring {{% tts %}} as `localhost` on a machine with no public IP or DNS address, see [Special Considerations - Localhost]({{< ref "getting-started/installation/configuration/localhost" >}}) section.

The `docker-compose.yml` file defines the Docker services of {{% tts %}} and its dependencies, and is used to configure Docker.

The configuration file `ttn-lw-stack-docker.yml` contains the configuration specific to {{% tts %}} deployment and is used to configure {{% tts %}}. When {{% tts %}} starts, it searches through `ttn-lw-stack-docker.yml` for a license key, hostname and other configuration parameters.

This guide assumes the following directory hierarchy:

```bash
docker-compose.yml          # defines Docker services for running {{% tts %}}
config/
└── stack/
    └── ttn-lw-stack-docker.yml    # configuration file for {{% tts %}}
```

### Example Configuration Files

{{< tabs/container "Enterprise" "Open Source" >}}
{{< tabs/tab "Enterprise" >}}

Download the example `docker-compose.yml` for {{% tts %}} Enterprise <a href="docker-compose-enterprise.yml" download="docker-compose.yml">here</a>.

Download the example `ttn-lw-stack-docker.yml` for {{% tts %}} Enterprise <a href="ttn-lw-stack-docker-enterprise.yml" download="ttn-lw-stack-docker.yml">here</a>.

{{< /tabs/tab >}}
{{< tabs/tab "Open Source" >}}

Download the example `docker-compose.yml` for {{% tts %}} Open Source <a href="docker-compose-open-source.yml" download="docker-compose.yml">here</a>.

Download the example `ttn-lw-stack-docker.yml` for {{% tts %}} Open Source <a href="ttn-lw-stack-docker-open-source.yml" download="ttn-lw-stack-docker.yml">here</a>.

{{< /tabs/tab >}}
{{< /tabs/container >}}

>**Note:** These example configuration files contain all of the configuration settings you need to run {{% tts %}} for development - just update the files with your server address. 

Settings in `docker-compose.yml` and `ttn-lw-stack-docker.yml` files are explained in detail in [Docker Configuration]({{< ref "getting-started/installation/configuration/docker-configuration" >}}) and [The Things Stack Configuration]({{< ref "getting-started/installation/configuration/the-things-stack-configuration" >}}) sections. Further, we provide tips for running {{% tts %}} in production.
