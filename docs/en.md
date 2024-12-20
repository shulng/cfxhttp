[简体中文](../README.md) | English  

Please help me improve this document.  

This script is used for deploying vless proxy on Cloudflare workers.  

#### Usage
 1. Pre-requirment: have a domain managed by Cloudflare.
 1. Enable `gRPC` feature in `network` settings in Cloudflare dashboard.
 1. Create a DNS `A record` for a new sub-domain with a random IPv4 address. Enable `proxy` option.
 1. Create a worker and copy-and-paste the source code from [src/index.js](../src/index.js).
 1. Goto worker's config panel, add a routing rule to your new sub-domain. e.g. `sub.your-website.com/*`.

*If you only use WebSocket transport, then only step 4 is required.*

There are some configurations at the top of the source code.  
 * `UUID` Need no explains.
 * `PROXY` (optional) Reverse proxy for websites using Cloudflare CDN. Format: `example.com`  
 * `LOG_LEVEL` debug, info, error, none  
 * `TIME_ZONE` Timestamp time zone of logs. e.g. Argentina is `-3`  
 * `XHTTP_PATH` URL path for xhttp transport. e.g. `/xhttp`. Leave it empty to disable this feature.
 * `WS_PATH` URL path for ws transport. e.g. `/ws`. Leave it empty to disable this feature.
 * `DOH_QUERY_PATH` URL path for DNS over HTTP(S) feature, e.g. `/doh-query`. Leave it empty to disable this feature.
 * `UPSTREAM_DOH` e.g. `https://dns.google/dns-query`. Do not use Cloudflare DNS.

You can setup those configurations in worker's enviroment variables config panel too. Env-vars have higher priority.  

If every thing goes right, you would see a `Hello world!` when accessing `https://sub.your-website.com/`.  
Visit `https://sub.your-website.com/(XHTTP_PATH)/?uuid=(YOUR-UUID)` to get a `client-config.json` with xhttp transport.  Replace `(XHTTP_PATH)` with `(WS_PATH)` to get a config with ws transport.  

#### Notice
 * This script is slow, do not expect too much.
 * Workers do not support UDP. Applications require UDP feature will not work. Such as DNS.
 * Workers have CPU executing-time limit. Applications require long-term connection would disconnect randomly. Such as downloading a big file.
 * DoH feature is not for xray-core, use DoT in `config.json` instead. e.g. `tcp://8.8.8.8:53`  
 * Enable one of ws transport or xhttp transport as needed. It's a bit wasteful to enable both.
 * The xhttp transport can only deploy on Cloudflare workers. [issue #2](https://github.com/vrnobody/cfxhttp/issues/2)
 * The more people knows of this script, the sooner this script got banned.

#### Credits
[tina-hello/doh-cf-workers](https://github.com/tina-hello/doh-cf-workers/) DoH feature  
[6Kmfi6HP/EDtunnel](https://github.com/6Kmfi6HP/EDtunnel/) WebSocket transport feature  
