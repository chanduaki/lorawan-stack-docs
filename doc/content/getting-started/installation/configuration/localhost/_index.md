---
title: "Special Considerations - Localhost"
description: ""
weight: 5
---

Follow this section if you are configuring and running {{% tts %}} on a local machine with no public IP or DNS address.

<!--more-->

`localhost` has a different meaning on your machine than inside the Docker container where {{% tts %}} runs, which can cause problems if you use `localhost` in your configuration.

`localhost` addresses on your machine will resolve to your machine, while `localhost` inside the Docker container will resolve inside the Docker container. In `docker-compose.yml`, we forward requests on ports 80 and 443 to ports 1885 and 8885 inside the container, so visiting `localhost` (which is really `localhost:80`) on your machine actually takes you to `localhost:1885` within the container.

If you configure your `is.oauth.ui.canonical-url` as `localhost`, this causes **Token Exchange Refused** errors when you try to log in to {{% tts %}} Console, because an authorization request generated from within the container will not be redirected to port 1885 or 8885, where {{% tts %}} is listening.

### Solution 1: Use the IP address of your computer on your local network

The best solution is to configure and use a static IP address for your machine on your local network so that redirects from your machine, from the Docker container, or from anywhere inside your local network, all resolve at the same place, on your machine. 

Follow instructions [here](https://uk.pcmag.com/news/124250/how-to-set-up-a-static-ip-address) for configuring a static IP address on your computer. Use that IP address as your server address, i.e replace `thethings.example.com` with that IP address. You may also generate a self-signed certificate for that IP address by following instructions in the [Certificates]({{< relref "certificates" >}}) section.

This will still allow you to see {{% tts %}} Console by entering `localhost` or your local IP address in your browser. It will also allow you to connect to {{% tts %}} from any machine inside your local network.

### Solution 2: Specify the internal ports that {{% tts %}} listens on in your configuration files

By default, {{% tts %}} listens on ports 1885 and 8885 inside Docker. To make `localhost` work both on your local machine and within the Docker container, append the port to `localhost` and make sure port forwarding is enabled in your `docker-compose.yml` for that port.

You must also remove ports 80 and 443 from your {{% tts %}} Docker configuration.

For example, 

```yaml
is:
  oauth:
    ui:
      canonical-url: 'https://thethings.example.com/oauth'
      is:
        base-url: 'https://thethings.example.com/api/v3'
```

becomes

```yaml
is:
  oauth:
    ui:
      canonical-url: 'https://localhost:8885/oauth'
      is:
        base-url: 'https://localhost:8885/api/v3'
``` 

and in `docker-compose.yml`, add the following port forwarding configuration (if it does not already exist), while removing ports 80 and 443:

```yaml
services:
  stack:
    ports:
      stack:
        - "1885:1885"
        - "8885:8885"
        # - "80:80" # Do not forward port 80
        # - "443:443" # Do not forward port 443
```

This will result in visits from both outside and inside the Docker container being received at the correct port by {{% tts %}}.
