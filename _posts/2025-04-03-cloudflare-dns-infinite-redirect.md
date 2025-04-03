---
layout: post
title: Fixing Cloudflare-Nginx Infinite Redirect Loops
lead: Encountered endless redirects after setting up Cloudflare with Nginx. This is why your SSL configuration creates this frustrating loop and how to solve it in two simple ways.
---
## Context

When integrating Cloudflare's DNS proxy with Nginx, you might encounter an unexpected infinite redirect loop. I recently faced this issue and want to document the cause and solution for future reference.

## The Problem

After setting up a proxied DNS record to my Nginx server, I encountered an endless 301 redirect loop. This happened because of a conflict between my Cloudflare SSL configuration choices.

### NGINX Config

As shown below every http request is redirected to https in my Nginx setup.

```nginx
server {
    listen          80;
    listen          [::]:80;
    server_name     ${PUBLIC_DOMAIN};

    return 301 https://${PUBLIC_DOMAIN}$request_uri;
}

server {
    listen          443 ssl default_server;
    ...
}
```

### Cloudflare SSL/TLS Settings

Cloudflare, when using its proxy, has a few SSL settings of its own.

 - `Off`: No encryption is applied. Https is disabled
 - `Flexible`: Encryption is enabled between the visitor and Cloudflare, however all connections between Cloudflare and your server are made through HTTP.
 - `Full`: Enables encryption end-to-end. Your certificate does not need to be a publicly trusted certificate.
 - `Full (Strict)`: Encryption is enabled end-to-end, and your certificate is validated.
 - `Strict (SSL-Only Origin Pull)`: Enforces encryption between Cloudflare and your server, regardless of your visitor's request.

 The default is "Flexible". This means that all requests are sent from Cloudflare to the server unencrypted.

## What Happens

The redirect loop occurs due to this sequence:

1. A visitor accesses the website via HTTP or HTTPS
2. Cloudflare proxy receives the request
3. With "Flexible" SSL mode enabled, Cloudflare forwards an HTTP request to Nginx (port 80)
4. Nginx follows its configuration and issues a 301 redirect to HTTPS (port 443)
5. Cloudflare receives this redirect and the cycle repeats endlessly

## Solution

You have two options to resolve this:

1. **Disable the 301 redirects on port 80** in your Nginx configuration, allowing HTTP traffic from Cloudflare

   OR

2. **Change Cloudflare's SSL/TLS encryption mode to "Full (Strict)"** which ensures Cloudflare connects to your origin server via HTTPS, maintaining end-to-end encryption

I personally went with the second option, as it ensures all traffic is encrypted throughout the entire journey from visitor to server.
If, for any reason, I switch to a different DNS provider in the future (without a Proxy), all http requests will still redirected to https.

Remember to clear your browser cache after making these changes to ensure you're seeing the updated behavior.
