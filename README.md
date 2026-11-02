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


To analyze the failed login attempts I did on the Security-test by running SPL searches. This included certain commands such as |stats count by Accounts_Name to look at how many events happened for each account name in the last 60 minutes.
<img width="1915" height="1112" alt="Screenshot 2026-11-01 184018" src="https://github.com/user-attachments/assets/cd077b83-7e95-4e97-ab8a-fed61fd29462" />
Running this SPL search made me learn that having the amount of counts by account name is flawed for finding out how much events happen for each account. The results show HANIFSLAPTOP$ and Security-test as two individual accounts with different amounts of counts of events for each. This could look like seven events from those two accounts, but there is an overlap because all of the events for Security-test happened on the HANIFSLAPTOP$ account because that represents my laptop. 

To isolate only the failed login attempts on the Security-test account, I had a search that included where Account_Name="Security-test" and |table Account_Name Failure_Reason. This search would be useful for analyst who already is suspicious of a specific account and wants to isolate the events only for that account. 
<img width="1917" height="1095" alt="Screenshot 2026-11-01 184859" src="https://github.com/user-attachments/assets/b5c3a17c-c58a-425a-bf83-10b8a6678edc" />



# 3. Detecting Repeated Failed Logins
After looking at the failed login attempts I created a search that identifies failed login attempts that happen three times in five minute intervals. As expected when I ran the search it detected three failed login attempts in the same five minute interval. Three failed login attempts in five minutes can mean someone is guessing passwords but not necessarily that it is from an attack. If this search had where count>=10 and it was detected, this would be pretty suspicious.
<img width="1917" height="1111" alt="Screenshot 2026-11-01 191314" src="https://github.com/user-attachments/assets/e3832e03-796d-495b-89a0-2b38012d57f0" />


# 4. Creating a Scheduled Alert 
I configured a scheduled Splunk alert from the previous detection search. This alert runs every five minutes and searches events from the past 5 minutes and is triggered when there is there is at least one matching result from the search.

<img width="987" height="915" alt="Screenshot 2026-11-01 192454" src="https://github.com/user-attachments/assets/2a192421-add9-4f2e-8899-1b0745e7251c" />
<img width="992" height="901" alt="Screenshot 2026-11-01 192637" src="https://github.com/user-attachments/assets/8b4ad79e-ed6a-452f-b8e3-379aad029fa6" />

# 5. Building a Security Monitoring Dashboard
I created a Splunk Dashboard that contains two bar graph visualizations. 
The one on the top called Failed Login Attempts Over Time shows the number of Event Id 4625 events grouped into five minute intervals.
The one at the bottom called Failed Login Attempts By Account Name shows the number of failed login events that happened to different accounts.
<img width="1917" height="1095" alt="Screenshot 2026-11-01 193727" src="https://github.com/user-attachments/assets/5483feaf-11a5-489e-b0c8-53361872bd93" />
As I explained in section 2, Windows events can have two different account name values, which is to be remembered for the second chart because three of the four failed login attempts for the HANIFSLAPTOP$ bar is the same event as the three login attempts for the Security-test bar.

# 6. Findings and Conclusion
I was able to successfully create three failed Windows login attempt events that was able to be collected by Splunk and analyzed. 
I learned SPL searches that would be useful for a SOC analyst monitoring Failed login attempt data trying to understand what happened.
I learned that windows events can have more than one account name values, which should be remembered for writing SPL searches grouping by Account_Name to understand that there can be an overlap.
I learned how to make a scheduled alert from a SPL search which runs automatically so a SOC analyst can detect patterns that seem malicious without having to do a SPL search for every sing time something happens.
I learned how to create a dashboard to visualize data to better understand it. 









