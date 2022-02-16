+++
title = "Browser Security"
draft = true
+++

Web browsers make some bad assumptions about security. Web servers can emit HTTP headers to help clarify restrictions.`Content-Security-Policy` is the current standard that encompasses security configuration, but there are some older standards we need to be aware of.

## X-Frame-Options
Explicitly controls if other websites can embed you in an `<iframe>`. Also known as `frame-ancestors` in `Content-Security-Policy`.

Most websites should just do this.

```http
X-Frame-Options: DENY
```

Plain and simple, don't allow embedding.

### Bypassing X-Frame-Options
It's just an HTTP header, so there's nothing stopping you from writing a [custom proxy server](https://medium.com/@alexcambose/bypassing-x-frame-options-4934dd852618) that filters out the header.

#### Resources
* <https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options>
* <https://www.rfc-editor.org/rfc/rfc7034#section-2>
* <https://stackoverflow.com/a/58788757>


## X-XSS-Protection

* <https://stackoverflow.com/a/57802070>

## CORS
TODO

## More

* <https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks>
* <https://www.keycdn.com/blog/http-security-headers>
* <https://caniuse.com/?search=content%20security%20policy>