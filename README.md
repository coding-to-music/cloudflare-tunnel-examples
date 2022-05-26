# cloudflare-tunnel-examples

Create a Tunnel with the name provided and associate it with a UUID. The relationship between the UUID and the name is persistent. The command will not create a connection at this point.

The created Tunnel can serve traffic for multiple hostnames in your Cloudflare account and send traffic to multiple services available to cloudflared, including SSH, RDP, and most arbitrary TCP connections.

Set up a tunnel locally (CLI setup)

1. Download and install cloudflared
   .deb install
   ​.rpm install
2. Authenticate cloudflared
3. Create a tunnel and give it a name
4. Create a configuration file
5. Start routing traffic
6. Run the tunnel
7. Check the tunnel

# 🚀 Javascript full-stack 🚀

https://github.com/coding-to-music/cloudflare-tunnel-examples

By Cloudflare Documentation

https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-useful-terms/#default-cloudflared-directory

https://developers.cloudflare.comhttps://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide/#set-up-a-tunnel-locally-cli-setup

https://developers.cloudflare.comhttps://developers.cloudflare.com/cloudflare-one/connections/connect-apps/create-tunnel/

https://github.com/cloudflare/cloudflare-docs/blob/production/contenthttps://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide.md

https://github.com/cloudflare/cloudflare-docs/blob/production/contenthttps://developers.cloudflare.com/cloudflare-one/connections/connect-apps/create-tunnel/index.md

## Environment Values

```java

```

## GitHub

```java
git init
git add .
git remote remove origin
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:coding-to-music/cloudflare-tunnel-examples.git
git push -u origin main
```

# Set up your first tunnel

https://github.com/cloudflare/cloudflare-docs/blob/production/contenthttps://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide.md

title: Set up your first tunnel

When setting up your first Cloudflare Tunnel, you have the option to create it:

- [Remotely on the Zero Trust dashboard](#set-up-a-tunnel-remotely-dashboard-setup)
- [Locally, using your CLI](#set-up-a-tunnel-locally-cli-setup)

## Prerequisites

Before you start, make sure you:

- [Add a website to Cloudflare](https://support.cloudflare.com/hc/en-us/articles/201720164-Creating-a-Cloudflare-account-and-adding-a-website).
- [Change your domain nameservers to Cloudflare](https://support.cloudflare.com/hc/en-us/articles/205195708).

First, download `cloudflared` on your machine. Visit the [downloads](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation/) page to find the right package for your OS.

Next, install `cloudflared`.

#### .deb install

Use the deb package manager to install `cloudflared` on compatible machines. `amd64 / x86-64` is used in this example.

```sh
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && dpkg -i cloudflared-linux-amd64.deb
```

#### ​.rpm install

Use the rpm package manager to install `cloudflared` on compatible machines. `amd64 / x86-64` is used in this example.

```sh
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-x86_64.rpm
```

</div>
</details>

<details>
<summary>Build from source</summary>
<div>

You can also build the latest version of `cloudflared` from source with the following steps.

```sh
git clone https://github.com/cloudflare/cloudflared.git
cd cloudflared
make cloudflared
go install github.com/cloudflare/cloudflared/cmd/cloudflared
```

Depending on where you installed `cloudflared`, you can move it to a known path as well.

```bash
mv /root/cloudflared/cloudflared /usr/bin/cloudflared
```

</div>
</details>

---

pcx-content-type: reference
title: Useful terms
weight: 6

---

# Useful terms

{{<render file="_cloudflared-new-ui.md">}}

Review terminology for tunnels setup locally through the CLI.

## Tunnel

A tunnel is a secure, outbound-only pathway you can establish between your origin and the Cloudflare edge. Each tunnel you create will be assigned a [name](#tunnel-name) and a [UUID](#tunnel-uuid).

## Tunnel UUID

A tunnel UUID is an alphanumeric, unique ID assigned to a tunnel. The tunnel UUID can be used in [configuration files](#configuration-file), and in general, whenever you need to reference a specific tunnel.

## Tunnel name

The `cloudflared tunnel create <NAME>` command creates a tunnel and assigns it a name. Once named, a tunnel is a persistent pathway within which you can stop and start as many [connectors](#connector) as needed, adding stability and ease of use to your tunnel experience. Tunnel names do not need to be hostnames; for example, you can assign your tunnel a name that represents your application/network, a particular server, or the cloud environment where it runs. Just choose any identifier that lets you easily reference a tunnel whenever you need.

## Connector

You can create and configure a tunnel once and run it as multiple different `cloudflared` processes. These processes are known as connectors, or replicas. DNS records and Cloudflare Load Balancers can still point to the tunnel and its UUID, while that tunnel sends traffic to the multiple instances of cloudflared that run through it. Using multiple connectors provides tunnels with high availability, scalability, and elasticity.

## Default `cloudflared` directory

`cloudflared` uses a default directory when storing credentials files for your tunnels, as well as the `cert.pem` file it generates when you run `cloudflared login`. The default directory is also where `cloudflared` will look for a [configuration file](#configuration-file) if no other file path is [specified](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/configuration/local-management/configuration-file/#storing-a-configuration-file) when running a tunnel.

| OS                          | Path to default directory                                                              |
| --------------------------- | -------------------------------------------------------------------------------------- |
| Windows                     | `%USERPROFILE%\.cloudflared`                                                           |
| MacOS and Unix-like systems | `~/.cloudflared`, `/etc/cloudflared`, and `/usr/local/etc/cloudflared`, in this order. |

## Configuration file

This is a `.yaml` file that functions as the operating manual for `cloudflared`. `cloudflared` will automatically look for the configuration file in the [default `cloudflared` directory](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-useful-terms/#default-cloudflared-directory), but you can store your configuration file in any directory. It is recommended to always specify the file path for your configuration file whenever you reference it. By creating a configuration file, you can have fine-grained control over how their instance of `cloudflared` will operate. This includes operations like what you want `cloudflared` to do with traffic (for example, proxy websockets to port `xxxx`, or ssh to port `yyyy`), where `cloudflared` should search for authorization (credentials file, tunnel token), and what mode it should run in (for example, [`warp-routing`](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/private-net/)). In the absence of a configuration file, cloudflared will proxy outbound traffic through port `8080`. For more information on how to create, store, and structure a configuration file, refer to the [dedicated instructions](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/configuration/local-management/configuration-file/).

## Ingress rule

[Ingress rules](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/configuration/local-management/ingress/) let you specify which local services traffic should be proxied to. If a rule does not specify a path, all paths will be matched. Ingress rules can be listed in your [configuration file](#configuration-file) or when running `cloudflared tunnel ingress`.

## Cert.pem

This is the certificate file issued by Cloudflare when you run `cloudflared tunnel login`. This file uses a certificate to authenticate your instance of `cloudflared` and it is required when you create new tunnels, delete existing tunnels, change DNS records, or configure tunnel routing from cloudflared. This file is not required to perform actions such as running an existing tunnel or managing tunnel routing from the Cloudflare dashboard. Refer to the [Tunnel permissions page](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-permissions/) for more details on when this file is needed.

The `cert.pem` origin certificate is valid for at least 10 years, and the service token it contains is valid until revoked.

## Credentials file

This file is created when you run `cloudflared tunnel create <NAME>`. It stores your tunnel’s credentials in JSON format, and is unique to each tunnel. This file functions as a token authenticating the tunnel it is associated with. Refer to the [Tunnel permissions page](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-permissions/) for more details on when this file is needed.

## Quick tunnels

Quick tunnels, when run, will generate a URL that consists of a random subdomain of the website `trycloudflare.com`, and point traffic to localhost on port 8080. If you have a web service running at that address, users who visit the generated subdomain will be able to visit your web service through Cloudflare’s network. Refer to [TryCloudflare](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/run-tunnel/trycloudflare/) for more information on how to run quick tunnels.

## Virtual Networks

A software abstraction that allows you to logically segregate resources on your private network. [Tunnel Virtual Networks](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/private-net/tunnel-virtual-networks/) are especially useful for exposing resources which have overlapping IP routes. To connect to a resource, end users would select a virtual network in their WARP client settings before entering the destination IP.

### 2. Authenticate `cloudflared`

```bash
cloudflared tunnel login
```

Running this command will:

- Open a browser window and prompt you to log in to your Cloudflare account. After logging in to your account, select your hostname.
- Generate an account certificate, the [cert.pem file](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-useful-terms/#cert-pem), in the [default `cloudflared` directory](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-useful-terms/#default-cloudflared-directory).

### 3. Create a tunnel and give it a name

```bash
cloudflared tunnel create <NAME>
```

Running this command will:

- Create a tunnel by establishing a persistent relationship between the [name you provide](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-useful-terms/#tunnel-name) and a [UUID](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-useful-terms/#tunnel-uuid) for your tunnel. At this point, no connection is active within the tunnel yet.
- Generate a [tunnel credentials file](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-useful-terms/#credentials-file) in the [default `cloudflared` directory](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-useful-terms/#default-cloudflared-directory).
- Create a subdomain of `.cfargotunnel.com`.

From the output of the command, take note of the tunnel’s UUID and the path to your tunnel’s credentials file.

Confirm that the tunnel has been successfully created by running:

```bash
cloudflared tunnel list
```

### 4. Create a configuration file

Create a [configuration file](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-useful-terms/#configuration-file) in your `.cloudflared` directory using any text editor. This file will configure the tunnel to route traffic from a given origin to the hostname of your choice.

Add the following fields to the file:

**If you are connecting an application**

```txt
url: http://localhost:8000
tunnel: <Tunnel-UUID>
credentials-file: /root/.cloudflared/<Tunnel-UUID>.json
```

**If you are connecting a network**

```txt
tunnel: <Tunnel-UUID>
credentials-file: /root/.cloudflared/<Tunnel-UUID>.json
warp-routing:
  enabled: true
```

Confirm that the configuration file has been successfully created by running:

```bash
cat config.yml
```

### 5. Start routing traffic

Now assign a CNAME record that points traffic to your tunnel subdomain.

**If you are connecting an application**

```bash
cloudflared tunnel route dns <UUID or NAME> <hostname>
```

**If you are connecting a network**

Add the IP/CIDR you would like to be routed through the tunnel.

```bash
cloudflared tunnel route ip add <IP/CIDR> <UUID or NAME>
```

You can confirm that the route has been successfully established by running:

```bash
cloudflared tunnel route ip show
```

### 6. Run the tunnel

Run the tunnel to proxy incoming traffic from the tunnel to any number of services running locally on your origin.

```bash
cloudflared tunnel run <UUID or NAME>
```

If your configuration file has a custom name or is not in the `.cloudflared` directory, add the `--config` flag and specify the path.

```sh
cloudflared tunnel --config /path/your-config-file.yaml run
```

{{<Aside>}}

Cloudflare Tunnel can install itself as a system service on Linux and Windows and as a launch agent on macOS. For more information, refer to [Run as a service](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/run-tunnel/as-a-service/).

{{</Aside>}}

### 7. Check the tunnel

Your tunnel configuration is complete! If you want to get information on the tunnel you just created, you can run:

```bash
cloudflared tunnel info
```

pcx-content-type: how-to

title: Create a Tunnel

# Create a Tunnel

| Before you start                                                                                                                                |
| ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. [Add a website to Cloudflare](https://support.cloudflare.com/hc/en-us/articles/201720164-Creating-a-Cloudflare-account-and-adding-a-website) |
| 2. [Change your domain nameservers to Cloudflare](https://support.cloudflare.com/hc/en-us/articles/205195708)                                   |
| 3. [Install and authenticate `cloudflared`](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/)       |

## Create a Tunnel

To create a Tunnel, run the following command:

```sh
cloudflared tunnel create <NAME>
```

Replace `<NAME>` with the name you want to give to the Tunnel. The name assigned can be any string and does not need to relate to the hostname where traffic will be served.

This command will create a Tunnel with the name provided and associate it with a UUID. The relationship between the UUID and the name is persistent. The command will not create a connection at this point.

The created Tunnel can serve traffic for multiple hostnames in your Cloudflare account and send traffic to multiple services available to `cloudflared`, including SSH, RDP, and most arbitrary TCP connections.

![Create a tunnel](https://github.com/coding-to-music/cloudflare-tunnel-examples/blob/main/images/ct1.png?raw=true)

Creating a Tunnel generates a credentials file for that specific Tunnel. This file is distinct from the cert.pem file. To run the Tunnel without managing DNS from `cloudflared`, you only need the credentials file.

{{<table-wrap>}}

| Action                                                              | `cert.pem` | Credentials file |
| ------------------------------------------------------------------- | ---------- | ---------------- |
| Create a new Tunnel                                                 | Required   | -                |
| Delete a Tunnel                                                     | Required   | -                |
| Run a Tunnel                                                        | Available  | Required         |
| Create DNS records<br/>from `cloudflared`                           | Required   | -                |
| Connect to load balancer<br/>pools from `cloudflared`               | Required   | -                |
| Route traffic to a running Tunnel<br/>from the Cloudflare dashboard | Available  | Available        |

{{</table-wrap>}}

## List available Tunnels

`cloudflared` can list all created Tunnels in your account, as well as those actively connected to Cloudflare, by running the following command:

```sh
cloudflared tunnel list
```

Note: the command requires the `cert.pem` file.

![List tunnels](https://github.com/coding-to-music/cloudflare-tunnel-examples/blob/main/images/lt1.png?raw=true)

## Revoke and delete a Tunnel

You can delete an existing Tunnel with cloudflared. To delete a Tunnel, run the following command:

```sh
cloudflared tunnel delete <NAME>
```

{{<Aside>}}

The command requires the `cert.pem` file.

{{</Aside>}}

If there are still active connections on that Tunnel, then you will have to force the deletion with:

```sh
cloudflared tunnel delete -f <NAME>
```

This will cause those connections to be dropped.

Deleting the Tunnel also invalidates the credentials file associated with that Tunnel, meaning those connections can not be re-established.

{{<Aside>}}

Tunnels created in this method do not currently display in the **Traffic** tab of the [Cloudflare dashboard](https://dash.cloudflare.com). These connections will be added to the dashboard in a future release.

{{</Aside>}}

Cloudflare Tunnel deletes DNS records after 24-48 hours of a Tunnel being unregistered. Cloudflare Tunnel does not delete TLS certificates on your behalf once the Tunnel is shut down. If you want to clean up a Tunnel you’ve shut down, you can delete DNS records [in the DNS editor](https://dash.cloudflare.com/?zone=dns) and revoke TLS certificates in the Origin Certificates section of the [SSL/TLS tab of the Cloudflare dashboard](https://dash.cloudflare.com?to=/:account/:zone/ssl-tls/origin).
