---
title: "Certificates"
description: ""
weight: 3
---

{{% tts %}} needs to be configured with Transport Layer Security (TLS) and HTTPS. This requires a TLS certificate and a corresponding key.

<!--more-->

In this guide, we show you how to request a free, trusted certificate from [Let's Encrypt](https://letsencrypt.org/getting-started/), but if you already have a certificate (`cert.pem`) and a corresponding key (`key.pem`), you can also use those. For local deployments, you can set up your own Certificate Authority and issue a certificate-key pair.

## Automatic Certificate Management (ACME)

{{% tts %}} can be configured to automatically retrieve and update Let's Encrypt certificates. Assuming you followed the [configuration]({{< relref "configuration" >}}) steps, create an `acme` directory where {{% tts %}} can store the certificate data:

```bash
$ mkdir ./acme
$ sudo chown 886:886 ./acme
```

> `886` is the uid and the gid of the user that runs {{% tts %}} in the Docker container. If you don't set these permissions, you'll get an error saying something like `open /var/lib/acme/acme_account+key<...>: permission denied`.

The directory hierarchy should now look like this:

```bash
acme/
docker-compose.yml          # defines Docker services for running {{% tts %}}
config/
└── stack/
    └── ttn-lw-stack-docker.yml    # configuration file for {{% tts %}}
```

## Certificates from a Certificate Authority

If you want to use the certificate (`cert.pem`) and key (`key.pem`) that you already have, you also need to set these permissions.

```bash
$ sudo chown 886:886 ./cert.pem ./key.pem
```

> If you don't set these permissions, you'll get an error saying something like `/run/secrets/key.pem: permission denied`.

The directory hierarchy should look like this:

```bash
cert.pem
key.pem
docker-compose.yml          # defines Docker services for running {{% tts %}}
config/
└── stack/
    └── ttn-lw-stack-docker.yml    # configuration file for {{% tts %}}
```

## Custom Certificate Authority

To use TLS on a local or offline deployment, you can use your own Certificate Authority. In order to set that up, you can use `cfssl`, CloudFlare's PKI/TLS toolkit. The `cfssl` installation instructions can be found [here](https://github.com/cloudflare/cfssl#installation).

Write the configuration for your CA to `ca.json`:

```json
{
  "names": [
    {"C": "NL", "ST": "Noord-Holland", "L": "Amsterdam", "O": "The Things Demo"}
  ]
}
```

Then use the following command to generate the CA key and certificate:

```bash
$ cfssl genkey -initca ca.json | cfssljson -bare ca
```

Now write the configuration for your certificate to `cert.json`:

```json
{
  "hosts": ["thethings.example.com"],
  "names": [
    {"C": "NL", "ST": "Noord-Holland", "L": "Amsterdam", "O": "The Things Demo"}
  ]
}
```

>**Note:** Remember to replace `thethings.example.com` with your server address!

Then, run the following command to generate the server key and certificate:

```bash
$ cfssl gencert -ca ca.pem -ca-key ca-key.pem cert.json | cfssljson -bare cert
```

The next steps assume the certificate key is called `key.pem`, so you'll need to rename `cert-key.pem` to `key.pem`.

At the end, your directory should look like this:

```bash
cert.pem
key.pem
ca.pem
docker-compose.yml          # defines Docker services for running {{% tts %}}
config/
└── stack/
    └── ttn-lw-stack-docker.yml    # configuration file for {{% tts %}}
```
