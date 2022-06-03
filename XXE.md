XML external entity injection (also known as XXE) is a web security vulnerability that allows an attacker to interfere with an application's processing of XML data. It often allows an attacker to view files on the application server filesystem, and to interact with any back-end or external systems that the application itself can access.

In some situations, an attacker can escalate an XXE attack to compromise the underlying server or other back-end infrastructure, by leveraging the XXE vulnerability to perform [server-side request forgery](https://portswigger.net/web-security/ssrf) (SSRF) attacks.

![](Pasted%20image%2020220603022145.png)

There are various types of XXE attacks:

-   [Exploiting XXE to retrieve files](https://portswigger.net/web-security/xxe#exploiting-xxe-to-retrieve-files), where an external entity is defined containing the contents of a file, and returned in the application's response.
-   [Exploiting XXE to perform SSRF attacks](https://portswigger.net/web-security/xxe#exploiting-xxe-to-perform-ssrf-attacks), where an external entity is defined based on a URL to a back-end system.
-   [Exploiting blind XXE exfiltrate data out-of-band](https://portswigger.net/web-security/xxe/blind#exploiting-blind-xxe-to-exfiltrate-data-out-of-band), where sensitive data is transmitted from the application server to a system that the attacker controls.
-   [Exploiting blind XXE to retrieve data via error messages](https://portswigger.net/web-security/xxe/blind#exploiting-blind-xxe-to-retrieve-data-via-error-messages), where the attacker can trigger a parsing error message containing sensitive data.

![](Pasted%20image%2020220603022409.png)

![](Pasted%20image%2020220603022455.png)

![](Pasted%20image%2020220603031510.png)

![](Pasted%20image%2020220603031525.png)

![](Pasted%20image%2020220603031548.png)

### How to Identify XML External Entity Vulnerabilities?
-   **File retrieval:** External entity, defined based on a well-known OS file, is used in the data obtained from the application’s response.
-   **Blind XXE vulnerabilities:** External entity is defined based on a URL to a system controlled by the tester/ developer and the interaction is monitored thereon.
-   **XInclude attacks:** Well-known OS file is attempted to be retrieved by testers leveraging XInclude attacks. Here, it is tested if user-supplied non-XML data is used within a server-side XML by the application.

### How to Mitigate XXE Vulnerabilities?

-   In most cases, XXE attacks can easily be prevented by disabling features making the XML processor weak and the application vulnerable. By analyzing the XML parsing library of the application, features that can be misused can be identified and disabled.
-   DTD and XML external entity features must be disabled.
-   All XML processors and libraries used in the application must be patched and updated always.
-   Ensure that the user inputs are validated before being parsed.
-   File uploads, server-side user inputs, and URLs must be sanitized, validated, and whitelisted.



