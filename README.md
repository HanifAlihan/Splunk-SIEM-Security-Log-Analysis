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


# 2. Investigating Failed Login Attempts
For this project because I am analyzing authentication logs, I focused on Event ID 4625 which represents a failed windows login attempt. To generate failed windows login attempts, I made a windows account called Security-test and then on purposely typed in the wrong password for this account 3 times. Then on Splunk I searched for failed login events with index=windows_security EventCode=4625.
<img width="1000" height="788" alt="Screenshot 2026-11-01 183121" src="https://github.com/user-attachments/assets/b4b95303-1a53-4efb-96db-737d3e49b1bc" />
<img width="945" height="196" alt="image" src="https://github.com/user-attachments/assets/c0237420-0f36-4e83-b0cd-b91e3e170447" />
<img width="1335" height="857" alt="Screenshot 2026-11-01 183454" src="https://github.com/user-attachments/assets/e2e73617-cfa1-4182-abac-0705c6be92ff" />


I expanded the > for the most recent at the top and examined fields such as Account_Name and Failure_Reason.
<img width="1917" height="1096" alt="Screenshot 2026-11-01 183610" src="https://github.com/user-attachments/assets/e14c804f-099f-4f23-bae3-c0156a3d9f36" />
<img width="1917" height="1086" alt="Screenshot 2026-11-01 183627" src="https://github.com/user-attachments/assets/36177d3a-6666-43cd-a034-a98b64a53e8b" />







