# Lab Write-ups: Security Misconfiguration

This directory stores completed, hands-on lab write-ups and vulnerability walkthroughs for **A02:2025 – Security Misconfiguration**.
CORS (Cross-Origin Resource Sharing) is a browser mechanism that controls whether JavaScript running on one origin can read a response from another origin.
To understand a CORS vulnerability, you first need to understand the browser rule it modifies: the Same-Origin Policy.

By default, web browsers are paranoid. If you are on website-A.com, the browser will not allow code on that site to pull personal data from website-B.com. This stops malicious websites from secretly reading your private data on other tabs you have open.

***CORS (Cross-Origin Resource Sharing)*** is a rulebook that tells the browser when it is safe to break that paranoid default rule. It acts as a VIP guest list. If website-B.com explicitly configures its CORS settings to say, "I trust website-A.com, let them access my data," the browser will allow it.
An origin is the combination of scheme, hostname, and port. For example, https://example.com and https://attacker.com are different origins.

This lab's vulnerability is that the server reflects an arbitrary origin supplied in the Origin request header, while also permitting credentialed requests.

A vulnerable response may look like this:
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Credentials: true


The first header tells the browser which origin is permitted to read the response. The second allows qualifying cross-origin requests to include credentials, such as the victim's session cookies.

Why is this dangerous? If the victim is logged in, an attacker-controlled website may be able to make a request using the victim's session and read private account information through JavaScript.
Lab 1: CORS vulnerability with basic origin reflection
APPENTICE
This website has an insecure CORS configuration in that it trusts all origins.
To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.
You can log in to your own account using the following credentials: wiener:peter
SOLUTION:
log in as weiner peter then in the history go to the /accountdetails page and capture that resquest to the repeater and add origin:https://example.com and then in response we see Access-Control-Allow-Origin now go to exploit server write the script in the body section and view exploit then deliver and then in access logs see for the api key of the administrator
Lab 2: CORS vulnerability with trusted null origin
Appentice
This website has an insecure CORS configuration in that it trusts the "null" origin.
To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.
You can log in to your own account using the following credentials: wiener:peter
SOLUTION:
login as weiner and then in http history go to accountdetails and send the request to repeater there add Origin:null and you will see  that the "null" origin is reflected in the Access-Control-Allow-Origin header.now go to exploit server and add the javascript then view exploit and access logs there we can get the administrators api key 
Lab 3: CORS vulnerability with trusted insecure protocols
PRACTITIONER
This website has an insecure CORS configuration in that it trusts all subdomains regardless of the protocol.
To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.
You can log in to your own account using the following credentials: wiener:peter
Step 1: Log in and inspect account details
1. Open the lab.
2. Turn Burp Proxy interception off.
3. Open Burp's browser.
4. Log in using:
Username: wiener
Password: peter


5. Open your account page.
6. In Burp HTTP history, find the request:
GET /accountDetails HTTP/2


This request retrieves the account information.

Send it to Repeater.
Step 2: Test the CORS configuration
In Repeater, add this header:
Origin: http://subdomain.YOUR-LAB-ID.web-security-academy.net


Replace YOUR-LAB-ID with your actual lab ID.

Send the request.

Look for these response headers:
Access-Control-Allow-Origin: http://subdomain.YOUR-LAB-ID.web-security-academy.net
Access-Control-Allow-Credentials: true


What does this tell us?

The server is reflecting the subdomain origin and allowing credentialed requests.

This is the first confirmation that the CORS configuration is vulnerable.
Step 3: Find the XSS vulnerability
Now we need a way to execute JavaScript on the insecure subdomain.
1. Return to the website.
2. Open any product page.
3. Click Check stock.
4. Observe the request and the URL used to load stock information.
You should find a request to a URL similar to:
http://stock.YOUR-LAB-ID.web-security-academy.net/
Notice that it uses HTTP, not HTTPS.
Now inspect the URL parameter:
productId=4
The lab's hint is telling you that this parameter is vulnerable to XSS.
In other words, the website does not properly handle input supplied through productId, allowing injected JavaScript to execute.
***CLICKJACKING***
Clickjacking (also known as UI redressing) is a digital magic trick where a hacker hides a dangerous button invisibly over a harmless-looking button. When you click what you think is a safe link, you are actually clicking the hidden, dangerous one.
A Simple Example
Imagine you take a piece of clear, transparent glass, paint a fake "Win a Free TV!" button on it, and hold it exactly over the "Send Money" button on a real ATM screen. When a victim reaches out to push the fake button on the glass, their finger actually presses the real "Send Money" button on the machine behind it.
On the internet, this looks like this:
The Setup: You visit a hacker's website called freeprizes.com. Right in the middle of the screen is a big, colorful button that says "Click to Claim Your TV!"
The Invisible Trap: The hacker uses a piece of web code (called an iframe) to load your real bank's website into a window on their page. However, they set the transparency of that window to 100% invisible.
The Alignment: The hacker carefully positions this invisible bank window so that the bank's real "Transfer $500 to Hacker" button hovers precisely over the visible "Claim Your TV!" button.
The Click: You click the button to claim your prize. But because the invisible bank window is layered on top, your mouse actually clicks the hidden bank transfer button. Since your browser remembers you are logged into your bank, the transfer goes through instantly.
Lab 1: Basic clickjacking with CSRF token protection
APPENTICE
This lab contains login functionality and a delete account button that is protected by a CSRF token. A user will click on elements that display the word "click" on a decoy website.
To solve the lab, craft some HTML that frames the account page and fools the user into deleting their account. The lab is solved when the account is deleted.
You can log in to your own account using the following credentials: wiener:peter
SOLUTION:
This is important because the lab title contains both terms.
CSRF	                                                    Clickjacking
Tricks a user's browser into sending an unwanted request.	Tricks a user into clicking an unintended element.
The attacker may submit a form or trigger a request.	    The attacker visually hides or disguises the real       interface.
CSRF tokens can help prevent it.	                        CSRF tokens alone do not necessarily prevent it.
User interaction may not be necessary.	                    Usually relies on the user clicking something.
But this lab already has CSRF protection. Why does clickjacking still work?
Normally, when you delete your account, the website checks a CSRF token.
A CSRF token is a random value generated by the website and associated with the user's session. It helps ensure that the request comes through an authorized workflow.
However, in this lab:
1. The user is already logged in.
2. The actual account page is loaded inside an iframe.
3. The browser sends the user's existing session cookies with the framed request, assuming the browser's cookie rules permit it.
4. The legitimate page already contains its valid CSRF token.
5. When the victim clicks Delete Account, the browser submits the legitimate form with the valid token.
The attacker does not need to steal or bypass the CSRF token. The victim's browser is interacting with the real website, and the legitimate page handles the token itself.
That is the central lesson of this lab.
 iframe
An iframe allows one webpage to display another webpage inside it.
 z-index
The z-index property determines which element appears above another element.
in the lab log into your account and then go to the exploit server and paste a html script in that script edit the values of width and height and to and side to fit the click me button just above the delete account button and then deliver the exploit and solve the lab.
Lab 2: Clickjacking with form input data prefilled from a URL parameter
APPENTICE
This lab extends the basic clickjacking example in Lab: Basic clickjacking with CSRF token protection. The goal of the lab is to change the email address of the user by prepopulating a form using a URL parameter and enticing the user to inadvertently click on an "Update email" button.
To solve the lab, craft some HTML that frames the account page and fools the user into updating their email address by clicking on a "Click me" decoy. The lab is solved when the email address is changed.
You can log in to your own account using the following credentials: wiener:peter
SOLUTION:
In this lab, the attack is almost identical, but there is one new feature:
We can prefill the email address field using a URL parameter.
Normally, the user enters a new email address and clicks Update email.

But suppose the website accepts an email address through its URL:
https://example.com/my-account?email=hacker@example.com
When the page loads, the email field is automatically populated with:
hacker@example.com
The attacker can take advantage of this feature.

The victim doesn't need to type anything. The attacker has already supplied the email address in the iframe URL.
2. What is the actual vulnerability?
There are two important things happening here.
1. Clickjacking
The attacker places the real account page inside an invisible iframe and positions a fake message over its Update email button.
2. URL parameter prepopulation
The website accepts an email address from the URL and automatically fills it into the email input field.
Together, these features allow an attacker to trick a logged-in victim into changing their email address.
the process is similar to the previous lab
Lab 3: Clickjacking with a frame buster script
This lab is protected by a frame buster which prevents the website from being framed. Can you get around the frame buster and conduct a clickjacking attack that changes the users email address?
To solve the lab, craft some HTML that frames the account page and fools the user into changing their email address by clicking on "Click me". The lab is solved when the email address is changed.
You can log in to your own account using the following credentials: wiener:peter
APPENTICE
SOLUTION:
A frame buster is JavaScript code used by a website to detect whether its page is being loaded inside an iframe.
For example, a website might use:
if (window.top !== window.self) {
    window.top.location = window.self.location;
}
Let's understand this:
- window.self refers to the current webpage.
- window.top refers to the top-level browser window.
- If both are different, the webpage is inside an iframe.
- The script attempts to escape the iframe by redirecting the top-level window to the website.
Why do websites use frame busters?

To prevent attackers from embedding sensitive pages inside their own websites and conducting clickjacking attacks.
However, frame busters implemented using JavaScript can sometimes be bypassed.
2. What is the new technique in this lab?
The important new attribute is:
sandbox="allow-forms"
This is an HTML iframe security attribute.
Normally, an iframe can be given permissions through the sandbox attribute. Without additional permissions, the browser restricts several actions inside that iframe.

For example, a sandboxed iframe without allow-scripts cannot execute JavaScript.

That is exactly what we need here.
The frame buster is JavaScript. If we prevent JavaScript execution inside the iframe, the frame buster cannot run.
But there is another problem: we still need the victim to submit the Update email form.
That's why we use:
sandbox="allow-forms"
This allows form submissions while continuing to block scripts.
SOLVE THE LAB IN THE SAME WAY.

