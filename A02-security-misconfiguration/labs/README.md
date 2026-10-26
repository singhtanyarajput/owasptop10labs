# Lab Write-ups: Security Misconfiguration

This directory stores completed, hands-on lab write-ups and vulnerability walkthroughs for **A02:2025 – Security Misconfiguration**.
CORS (Cross-Origin Resource Sharing) is a browser mechanism that controls whether JavaScript running on one origin can read a response from another origin.

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

