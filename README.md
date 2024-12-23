# CCTV-Logs-Investigation
Project investigating unauthorized access to CCTV logs using Splunk

Investigating CCTV Logs in Splunk

Project Overview
The project aims to investigate an incident involving unauthorized access to a CCTV system and subsequent deletion of footage. Using Splunk as the primary Security Information and Event Management (SIEM) tool, the project demonstrates a step-by-step methodology to parse, analyze, and correlate logs to identify the attacker.

Objectives
Understand and analyze poorly formatted logs.
Extract meaningful fields using regular expressions.
Identify suspicious activities and correlate data across datasets.
Pinpoint the timeline of the attack and the identity of the attacker.

Tools Used
Splunk: A platform for searching, monitoring, and analyzing machine-generated data.
Regex: Custom patterns for extracting meaningful data from raw logs.

Investigation Workflow
 
Step 1: Understanding the Logs
Datasets:
web_logs: Captures web server activity.
cctv_logs: Includes events like logins, downloads, and deletions.
Initial query to display all logs: index=*.

Step 2: Parsing the Logs
Problem: Logs were unreadable in their raw format.
Solution: Created a custom regex to extract critical fields such as timestamp, event type, user ID, username, and session ID.
Regex Pattern:

^(?P<timestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}) (?P<Event>\w+( \w+)?) (?P<user_id>\d+)? (?P<UserName>\w+) .*?(?P<Session_id>\w+)$
Used Splunk's Field Extraction tool for implementation.

Step 3: Analyzing CCTV Logs
Key queries:
Count of events by user: index=cctv_feed | stats count(Event) by UserName.
Rare events: index=cctv_feed | rare Event.
Failed logins: index=cctv_feed failed | table _time UserName Event Session_id.
Deleted footage: index=cctv_feed Delete.

Step 4: Correlating with Web Logs
Matched session IDs from cctv_logs with web_logs to identify IP addresses and trace activities.
Example query:
index=web_logs clientip="10.11.105.33" | table _time clientip status uri ur_path file.

Step 5: Identifying the Attacker
Correlated session IDs and IP addresses with cctv_feed logs.
Traced back to identify the username and timeline of events.

Summary of Findings
The attacker used brute force to gain access.
Successfully logged in, viewed, downloaded streams, and deleted footage.
IP address and associated session IDs helped trace the attacker’s identity.

Conclusion
This project demonstrates a comprehensive approach to investigating cybersecurity incidents using Splunk. By leveraging log parsing, data visualization, and correlation, it is possible to uncover hidden patterns and identify malicious activities
