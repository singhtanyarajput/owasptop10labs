# Lab Write-ups: Security Misconfiguration

This directory stores completed, hands-on lab write-ups and vulnerability walkthroughs for **A02:2025 – Security Misconfiguration**.
CORS (Cross-Origin Resource Sharing) is a browser mechanism that controls whether JavaScript running on one origin can read a response from another origin.
To understand a CORS vulnerability, you first need to understand the browser rule it modifies: the Same-Origin Policy.

By default, web browsers are paranoid. If you are on website-A.com, the browser will not allow code on that site to pull personal data from website-B.com. This stops malicious websites from secretly reading your private data on other tabs you have open.

CORS (Cross-Origin Resource Sharing) is a rulebook that tells the browser when it is safe to break that paranoid default rule. It acts as a VIP guest list. If website-B.com explicitly configures its CORS settings to say, "I trust website-A.com, let them access my data," the browser will allow it.
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


