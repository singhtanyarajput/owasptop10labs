# Lab Write-ups: Broken Access Control

This directory stores completed, hands-on lab write-ups and vulnerability walkthroughs for **A01:2025 – Broken Access Control**.
Lab 1: User role controlled by request parameter
APPENTICE
This lab has an admin panel at /admin, which identifies administrators using a forgeable cookie.
Solve the lab by accessing the admin panel and using it to delete the user carlos.
You can log in to your own account using the following credentials: wiener:peter
SOLUTION:
Browse to /admin and observe that you can't access the admin panel.
Browse to the login page.
In Burp Proxy, turn interception on and enable response interception.
Complete and submit the login page, and forward the resulting request in Burp.
Observe that the response sets the cookie Admin=false. Change it to Admin=true.
Load the admin panel and delete carlos.
Lab 2: User role can be modified in user profile
APPENTICE
This lab has an admin panel at /admin. It's only accessible to logged-in users with a roleid of 2.
Solve the lab by accessing the admin panel and using it to delete the user carlos.
You can log in to your own account using the following credentials: wiener:peter
SOLUTION:
send the update email request with inercept on and then send the POST request to repeater thereadd "roleid":=2 and then send it if it shows 302 redirect request then change the same thing in intercept area and send it in browser you will see admin panel added click and delete user carlos 
Lab 3: User ID controlled by request parameter
APPENTICE
This lab has a horizontal privilege escalation vulnerability on the user account page.
To solve the lab, obtain the API key for the user carlos and submit it as the solution.
You can log in to your own account using the following credentials: wiener:peter
SOLUTION:
login as weiner and then go to my account send the request to repeater and change id=weiner to carlos and send the request the api for carlos is accessible.
Lab 4: User ID controlled by request parameter, with unpredictable user IDs
APPENTICE
This lab has a horizontal privilege escalation vulnerability on the user account page, but identifies users with GUIDs.
To solve the lab, find the GUID for carlos, then submit his API key as the solution.
You can log in to your own account using the following credentials: wiener:peter
SOLUTION:
find a blog post by user carlos and click on her name to find the user id than log into your account and replace the user id to get access to carlos's API key.
its horizontal priviledge acceleration.
Lab 5: User ID controlled by request parameter with data leakage in redirect 
APPENTICE
This lab contains an access control vulnerability where sensitive information is leaked in the body of a redirect response.
To solve the lab, obtain the API key for the user carlos and submit it as the solution.
You can log in to your own account using the following credentials: wiener:peter
APPENTICE
login as weiner and then go to my account and send that request to repeater and there change id= weiner to carlos and in the response you will find 302 redirect with the API key in the html response 
Lab 6: User ID controlled by request parameter with password disclosure
APPENTICE
This lab has user account page that contains the current user's existing password, prefilled in a masked input.
To solve the lab, retrieve the administrator's password, then use it to delete the user carlos.
You can log in to your own account using the following credentials: wiener:peter
SOLUTION:
login as weiner and then send the my account request to repeater and in the response we can see the password now change id to administrator and in html response we get the password use the username and password to login to the admin panel and delete the user carlos
Lab 7: Insecure direct object references
APPENTICE
This lab stores user chat logs directly on the server's file system, and retrieves them using static URLs.
Solve the lab by finding the password for the user carlos, and logging into their account.
SOLUTION:
go to live chat section and now go to inspect and network tab now send a msg hello and click view tanscript a .txt file gets downloaded in the network tab we can see the url of that file change it to 1.xt and browser it then we get the password of the user in the text file which can be used to login into carlos's account and solve the labs.
Lab 8: Method-based access control can be circumvented
PRACTITIONER
This lab implements access controls based partly on the HTTP method of requests. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.
To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator.
understanding lab:this lab deals with priviledge escalation means a normal user can do admin functions we deal with the http request parameters to check if change in the parameters allows a particular action that is not supposed to be done by that user the http request is checked inconsistently or not 
SOLUTION:
to solve later some problem is coming*****
some pics i have added i tried solving the lab but some error is coming
Lab 9: Multi-step process with no access control on one step
PRACTITIONER
This lab has an admin panel with a flawed multi-step process for changing a user's role. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.
To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator.
SOLUTION:
login as admin and try promoting carlos capture the confirmation request final step into repeater and then open an incognito mode browser login as non-admin user and then find the cookie in inspect-> application-> session copy the cookie and replace the admin cookie in the repeater with non-admin user's cookie then change the name of user in request parameter from carlos to wiener and send the request.
Lab 10: Referer-based access control
PRACTITIONER
This lab controls access to certain admin functionality based on the Referer header. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.
To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator.
SOLUTION:
same things only as above 
*******Vulnerability: Referer-based access control bypass.

Root cause: The application relies on a client-controlled HTTP header instead of enforcing proper authorization.

Impact: A normal user may gain administrator privileges.

Testing technique: Capture a legitimate role-change request, replay it using a normal user's session, and test whether the Referer header influences authorization.

Remediation: Enforce authorization server-side for every sensitive operation. Never treat Referer or other client-controlled headers as proof of privilege.
***How this differs from your previous labs***
Lab	Main weakness
Method-based access control	Authorization differs depending on the HTTP method.
Multi-step process	One step lacks an authorization check.
Referer-based access control	Authorization relies on a client-controlled Referer header.


