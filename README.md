# Splunk-SIEM-Security-Log-Analysis
This project uses Splunk Enterprise for analyzing Windows authentication logs and looking at failed login attempts.

# Project Overview
In this project I used Splunk Enterprise to collect and analyze windows security event logs on my Laptop using Windows 11. The purpose of this project is to gain hands on experience using a SIEM tool like Splunk for security monitoring by investigating failed login attempts and trying to identify malicious activity.

# Tools used
-Splunk Enterprise 10.6.1.1
-OS: Windows 11
-Splunk Search Processing Language (SPL)

# 1. Setting up Splunk Enterprise
I made an account for Splunk and installed Splunk Enterprise 10.6.1.1 for windows. 
<img width="1917" height="937" alt="image" src="https://github.com/user-attachments/assets/dd3ab711-2648-419e-8bfc-f9625688bbfb" />



After Splunk Enterprise finished installing, it opened on a Microsoft Edge browser. I configured it for collecting windows security event logs by clicking add data on the home page, choosing monitor for the method of getting data, choosing local event logs, choosing security events on my laptop and then creating an index called "windows_security".
<img width="1886" height="1092" alt="Screenshot 2026-11-01 180120" src="https://github.com/user-attachments/assets/a3981db8-7ced-4e90-afb3-65140233be48" />
<img width="1917" height="1097" alt="Screenshot 2026-11-01 180204" src="https://github.com/user-attachments/assets/0cd4e39b-ef5d-4ff8-8ce0-005ad496216a" />



I made sure the Windows Security events was collected by Splunk I ran index=windows_security. It showed 33,667 events and it confirmed for me that the security events were being collected.
<img width="1917" height="1102" alt="Screenshot 2026-11-01 180428" src="https://github.com/user-attachments/assets/5ae6a0f6-bf1f-4c4a-b035-a1aac5045cd1" />






