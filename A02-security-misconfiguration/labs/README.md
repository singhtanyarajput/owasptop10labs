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

