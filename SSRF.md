## What is SSRF?

Server-side request forgery (also known as SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make requests to an unintended location.

In a typical SSRF attack, the attacker might cause the server to make a connection to internal-only services within the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems, potentially leaking sensitive data such as authorization credentials.

![](Pasted%20image%2020220601110600.png)

Types of attacks

1. Basic SSRF
`POST /product/stock HTTP/1.0 Content-Type: application/x-www-form-urlencoded Content-Length: 118 stockApi=http://localhost/admin`

### SSRF with blacklist-based input filters

Some applications block input containing hostnames like `127.0.0.1` and `localhost`, or sensitive URLs like `/admin`. In this situation, you can often circumvent the filter using various techniques:

-   Using an alternative IP representation of `127.0.0.1`, such as `2130706433`, `017700000001`, or `127.1`.
-   Registering your own domain name that resolves to `127.0.0.1`. You can use `spoofed.burpcollaborator.net` for this purpose.
-   Obfuscating blocked strings using URL encoding or case variation.

### SSRF with whitelist-based input filters

Some applications only allow input that matches, begins with, or contains, a whitelist of permitted values. In this situation, you can sometimes circumvent the filter by exploiting inconsistencies in URL parsing.

-   You can embed credentials in a URL before the hostname, using the `@` character. For example:
    
    `https://expected-host@evil-host`
-   You can use the `#` character to indicate a URL fragment. For example:
    
    `https://evil-host#expected-host`
-   You can leverage the DNS naming hierarchy to place required input into a fully-qualified DNS name that you control. For example:
    
    `https://expected-host.evil-host`
	
	
### Using Other Schemas

Apart from the _http://_ and _https://_ URL schemas, an attacker may take advantage of lesser-known or legacy URL schemas to access files on the local system or on the internal network.

The following example uses the _file:///_ URL schema.

```
GET /?url=file:///etc/passwd HTTP/1.1
Host: example.com
```

## Mitigating server-side request forgery
- Whitelists and DNS resolution

	The most robust way to avoid server-side request forgery (SSRF) is to whitelist the hostname (DNS name) or IP address that your application needs to access. If a whitelist approach does not suit you and you must rely on a blacklist,
	
- Disable unused URL schemas

If your application only uses HTTP or HTTPS to make requests, allow only these URL schemas. If you disable unused URL schemas, the attacker will be unable to use the web application to make requests using potentially dangerous schemas such as _file:///_, _dict://_, _ftp://_, and _gopher://_.

- Authentication on internal services

By default, services such as Memcached, Redis, Elasticsearch, and MongoDB do not require authentication. An attacker can use server-side request forgery vulnerabilities to access some of these services without any authentication. Therefore, to protect your sensitive information and ensure web application security, it’s best to enable authentication wherever possible, even for services on the local network.