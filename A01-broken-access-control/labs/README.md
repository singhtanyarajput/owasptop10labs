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
This lab has a horizontal privilege escalation vulnerability on the user account page.
To solve the lab, obtain the API key for the user carlos and submit it as the solution.
You can log in to your own account using the following credentials: wiener:peter
SOLUTION:
login as weiner and then go to my account send the request to repeater and change id=weiner to carlos and send the request the api for carlos is accessible.

