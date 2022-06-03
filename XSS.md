**Types of XSS-**
- Reflected :- [Reflected XSS](https://portswigger.net/web-security/cross-site-scripting/reflected) is the simplest variety of cross-site scripting. It arises when an application receives data in an HTTP request and includes that data within the immediate response in an unsafe way.
- Stored :- [Stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored) (also known as persistent or second-order XSS) arises when an application receives data from an untrusted source and includes that data within its later HTTP responses in an unsafe way.
- DOM :- DOM-based XSS (also known as [DOM XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based)) arises when an application contains some client-side JavaScript that processes data from an untrusted source in an unsafe way, usually by writing the data back to the DOM.

In the following example, an application uses some JavaScript to read the value from an input field and write that value to an element within the HTML:

`var search = document.getElementById('search').value; var results = document.getElementById('results'); results.innerHTML = 'You searched for: ' + search;`

If the attacker can control the value of the input field, they can easily construct a malicious value that causes their own script to execute:

`You searched for: <img src=1 onerror='/* Bad stuff here... */'>`

**Impact**
-   Impersonate or masquerade as the victim user.
-   Carry out any action that the user is able to perform.
-   Read any data that the user is able to access.
-   Capture the user's login credentials.
-   Perform virtual defacement of the web site.
-   Inject trojan functionality into the web site.

**Remediations**
-   **Filter input on arrival.** At the point where user input is received, filter as strictly as possible based on what is expected or valid input.
-   **Encode data on output.** At the point where user-controllable data is output in HTTP responses, encode the output to prevent it from being interpreted as active content. Depending on the output context, this might require applying combinations of HTML, URL, JavaScript, and CSS encoding.
-   **Use appropriate response headers.** To prevent XSS in HTTP responses that aren't intended to contain any HTML or JavaScript, you can use the `Content-Type` and `X-Content-Type-Options` headers to ensure that browsers interpret the responses in the way you intend.
-   **Content Security Policy.** As a last line of defense, you can use Content Security Policy (CSP) to reduce the severity of any XSS vulnerabilities that still occur.
-   **Use of HttpOnly Flag** The HttpOnly flag is a useful prevention mechanism, as it instructs the user-agent to restrict access to the cookie only for use with HTTP messages.As a result, even if an [XSS](https://www.whitehatsec.com/glossary/content/cross-site-scripting) flaw exists and a user accidentally accesses a link that exploits this flaw, the browser will not reveal the cookie to a third party.

**CSP**
Your policy should include a [`default-src`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) policy directive, which is a fallback for other resource types when they don't have policies of their own (for a complete list, see the description of the [`default-src`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) directive). A policy needs to include a [`default-src`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) or [`script-src`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) directive to prevent inline scripts from running, as well as blocking the use of `eval()`. A policy needs to include a [`default-src`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) or [`style-src`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/style-src) directive to restrict inline styles from being applied from a [`<style>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/style) element or a `style` attribute.


