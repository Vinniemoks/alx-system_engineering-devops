Postmortem: Outage of Web Application Due to Database Connection Pool Exhaustion
Issue Summary
•	Duration of Outage: 2 hours, from 10:00 AM to 12:00 PM UTC on October 10, 2023.
•	Impact: The web application was completely unresponsive for 100% of users. Users experienced HTTP 500 errors when attempting to access the service. This affected all users globally, as the database connection pool exhaustion prevented any requests from being processed.
•	Root Cause: The database connection pool was exhausted due to a sudden spike in traffic combined with a misconfigured connection pool size limit.
________________________________________
Timeline
•	10:15 AM: Issue detected when monitoring alerts flagged a spike in HTTP 500 errors and a drop in successful requests.
•	10:25 AM: Engineers noticed the database connection pool was maxed out, with all connections in use.
•	10:40 AM: Assumed the issue was caused by a recent code deployment and rolled back the deployment. No improvement observed.
•	10:50 AM: Investigated database performance metrics, suspecting slow queries or locks. No anomalies found.
•	10:55 AM: Escalated to the database and infrastructure teams to investigate connection pool behavior.
•	11:23 AM: Identified the root cause: the connection pool size was too small to handle the traffic spike.
•	11:49 AM: Increased the database connection pool size and restarted the application servers.
•	12:20 PM: Service fully restored.
________________________________________
Root Cause and Resolution
•	Root Cause: The database connection pool was configured with a maximum of 50 connections, which was insufficient to handle a sudden 3x traffic spike caused by a marketing campaign. This led to all connections being exhausted, causing the application to fail to serve requests.
•	Resolution: The connection pool size was increased to 200, and the application servers were restarted to apply the new configuration. Additionally, the database was optimized to close idle connections faster.
________________________________________
Corrective and Preventative Measures
•	Improvements/Fixes:
1.	Review and adjust connection pool settings to handle traffic spikes.
2.	Implement auto-scaling for the application servers during high traffic.
3.	Add better monitoring for connection pool usage and database performance.
4.	Conduct load testing to identify system limits before major campaigns.
•	TODO List:
1.	Patch the application server configuration to increase the default connection pool size.
2.	Add a monitoring alert for database connection pool usage (e.g., alert if >80% of connections are in use).
3.	Implement auto-scaling rules for the application servers based on traffic thresholds.
4.	Schedule load testing before the next major marketing campaign.
5.	Document the incident and share learnings with the engineering team.


