## What is CSRF?

Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.

![](Pasted%20image%2020220603064221.png)

## What is the impact of a CSRF attack?

In a successful CSRF attack, the attacker causes the victim user to carry out an action unintentionally. For example, this might be to change the email address on their account, to change their password, or to make a funds transfer. Depending on the nature of the action, the attacker might be able to gain full control over the user's account. If the compromised user has a privileged role within the application, then the attacker might be able to take full control of all the application's data and functionality.

## How does CSRF work?

For a CSRF attack to be possible, three key conditions must be in place:

-   **A relevant action.** There is an action within the application that the attacker has a reason to induce. This might be a privileged action (such as modifying permissions for other users) or any action on user-specific data (such as changing the user's own password).
-   **Cookie-based session handling.** Performing the action involves issuing one or more HTTP requests, and the application relies solely on session cookies to identify the user who has made the requests. There is no other mechanism in place for tracking sessions or validating user requests.
-   **No unpredictable request parameters.** The requests that perform the action do not contain any parameters whose values the attacker cannot determine or guess. For example, when causing a user to change their password, the function is not vulnerable if an attacker needs to know the value of the existing password.

![](Pasted%20image%2020220603064425.png)

### Mitigation 
CSRF attacks are only possible because cookies are always sent with any requests that are sent to a particular origin related to that cookie (see the definition of the [same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)). You can set a flag for a cookie that turns it into a same-site cookie. A same-site cookie is a cookie that can only be sent if the request is being made from the origin related to the cookie (not cross-domain). The cookie and the request source are considered to have the same origin if the protocol, port (if applicable) and host (but not the IP address) are the same for both.
[[SameSite Cookies]]

The most popular method to prevent Cross-site Request Forgery is to use a challenge token that is associated with a particular user and that is sent as a hidden value in every state-changing form in the web app. This token, called an _anti-CSRF token_ (often abbreviated as _CSRF token_) or a _synchronizer token_, works as follows:

-   The web server generates a token and stores it
-   The token is statically set as a hidden field of the form
-   The form is submitted by the user
-   The token is included in the POST request data
-   The application compares the token generated and stored by the application with the token sent in the request
-   If these tokens match, the request is valid
-   If these tokens do not match, the request is invalid and is rejected