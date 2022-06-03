# Defending against CSRF with SameSite cookies

Some web sites defend against [CSRF attacks](https://portswigger.net/web-security/csrf) using SameSite cookies.

The `SameSite` attribute can be used to control whether and how cookies are submitted in cross-site requests. By setting the attribute on session cookies, an application can prevent the default browser behavior of automatically adding cookies to requests regardless of where they originate.

The `SameSite` attribute is added to the `Set-Cookie` response header when the server issues a cookie, and the attribute can be given two values, `Strict` or `Lax`. For example:

`Set-Cookie: SessionId=sYMnfCUrAlmqVVZn9dqevxyFpKZt30NN; SameSite=Strict;` `Set-Cookie: SessionId=sYMnfCUrAlmqVVZn9dqevxyFpKZt30NN; SameSite=Lax;`

If the `SameSite` attribute is set to `Strict`, then the browser will not include the cookie in any requests that originate from another site. This is the most defensive option, but it can impair the user experience, because if a logged-in user follows a third-party link to a site, then they will appear not to be logged in, and will need to log in again before interacting with the site in the normal way.

If the `SameSite` attribute is set to `Lax`, then the browser will include the cookie in requests that originate from another site but only if two conditions are met:

-   The request uses the GET method. Requests with other methods, such as POST, will not include the cookie.
-   The request resulted from a top-level navigation by the user, such as clicking a link. Other requests, such as those initiated by scripts, will not include the cookie.

Using `SameSite` cookies in `Lax` mode does then provide a partial defense against CSRF attacks, because user actions that are targets for CSRF attacks are often implemented using the POST method. Two important caveats here are:

-   Some applications do implement sensitive actions using GET requests.
-   Many applications and frameworks are tolerant of different HTTP methods. In this situation, even if the application itself employs the POST method by design, it will in fact accept requests that are switched to use the GET method.

For the reasons described, it is not recommended to rely solely on SameSite cookies as a defense against CSRF attacks. Used in conjunction with [CSRF tokens](https://portswigger.net/web-security/csrf/tokens), however, SameSite cookies can provide an additional layer of defense that might mitigate any defects in the token-based defenses.