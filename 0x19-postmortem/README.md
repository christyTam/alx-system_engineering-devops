Database Connectivity Outage: Postmortem

Issue Summary

Duration of Outage: The outage lasted for approximately 3 hours, from 2:00 AM to 5:00 AM UTC  13th August , 2024.
Impact: 70% of users were not able to access the web application when the outage occured. Service experienced significant slowdowns and timeouts, leading to widespread user frustration and an 80% drop in transaction volume during the period.

Root Cause: The root cause of the outage was an exhausted connection pool in the PostgreSQL database, which was triggered by a misconfiguration in the database connection settings within the application’s server.
The database connection pool was like a crowded party where nobody could get in... and sadly, no one was having fun.


Timeline

2:00 AM: Monitoring alerts detected a spike in response times and database connection errors. Users started reporting issues accessing the service.

2:05 AM: The on-call engineer was notified via automated alert systems. Initial investigation pointed to a possible network issue.

2:15 AM: The network team was consulted to verify if there were any ongoing connectivity issues. No network problems were found.

2:30 AM: Database logs were analyzed, revealing a large number of failed connection attempts. It was assumed that the database server might be overwhelmed, so the database team was brought in to investigate.

3:00 AM: The database team confirmed that the database server was operational, but connection attempts were being rejected due to an exhausted connection pool.

3:15 AM: The application server logs were reviewed, revealing that a recent update had introduced a misconfiguration in the database connection pool settings, significantly lowering the available connections.

3:30 AM: The configuration was corrected, and the application server was restarted. Connection to the database was restored, and normal service resumed.

5:00 AM: Full service was restored, and monitoring confirmed that the issue was fully resolved.

Root Cause and Resolution
Root Cause: The issue was caused by a misconfiguration introduced during a recent deployment. Specifically, the database connection pool settings were altered, reducing the maximum number of available connections from 100 to 10. This change led to the connection pool being quickly exhausted, preventing new connections from being established and causing the application to fail to communicate with the database.
Resolution: The configuration was corrected by updating the connection pool settings to allow the appropriate number of connections (restored to the original 100 connections). The application server was then restarted, allowing it to reconnect to the database, and normal operations resumed.


Corrective and Preventative Measures
Improvements:
Implement stricter configuration review processes before deployment.
Increase monitoring around database connection pools to detect exhaustion earlier.
Conduct regular audits of database and server configurations.


Tasks:
Patch Application Configuration: Ensure that the correct database connection settings are applied across all environments.
Add Connection Pool Monitoring: Implement monitoring to track the usage of the database connection pool and set alerts for when usage reaches critical thresholds.
Review Deployment Processes: Introduce a mandatory review step for all configuration changes, including automated tests to verify settings before deploying to production.
Increase On-Call Training: Provide additional training for on-call engineers to better diagnose connection-related issues and understand the significance of database pool configurations.


