+++
title = "NginX"
+++

* Tuning NginX: <https://github.com/denji/nginx-tuning>
  * Discussion about the above: <https://gist.github.com/denji/8359866>
  * _"gzip will switch sendfile off! Because gzip is a content filter, and nginx will do the compression for every ... requests! not good at all, never use content filter if you care about performance."_
* Using only TLS 1.2 and 1.3: <https://www.cyberciti.biz/faq/configure-nginx-to-use-only-tls-1-2-and-1-3/>
