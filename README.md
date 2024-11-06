# HIDS
Host-based Intrusion Detection System
<br>
This project demonstrates the setup and configuration of a Host-based Intrusion Detection System (HIDS) using OSQuery on Kali Linux. It includes automated query scheduling for real-time monitoring and logging of system activities to detect suspicious behavior. Logs and configurations are optimized to track file integrity, monitor processes, and audit network activity, providing foundational insight into HIDS operations. This project exemplifies the use of open-source tools in a practical cybersecurity scenario, with configurations designed for efficiency and ease of deployment.

# Install OSQuery
1. **Open Terminal in Kali.**
2. Run the following commands to install OSQuery:
```
sudo apt update
sudo apt install osquery -y
```
3. After installation, check if OSQuery is installed by running:

```
osqueryi --version

```

<img src="https://github.com/user-attachments/assets/abb845f4-5321-445a-9c46-5186f9d30180" width="434" height="800">
<br>

<br>
Testing OSQuery in interactive shell: 

**SELECT * FROM os_version;**
<br>
<img src="https://github.com/user-attachments/assets/13a291c3-6022-4888-9c9f-0f3b4a663a2a" width="634" height="250">

# Create Configuration Files

```
sudo mkdir -p /etc/osquery
```
<br>
Create osquery.conf

```
sudo nano /etc/osquery/osquery.conf
```
Adding configuration into the created file

```
json
Copy code
{
  "options": {
    "config_plugin": "filesystem",
    "logger_plugin": "filesystem",
    "schedule_splay_percent": 10,
    "log_result_events": "true",
    "worker_threads": 2
  },
  "schedule": {
    "file_events": {
      "query": "SELECT * FROM file_events;",
      "interval": 60
    },
    "processes": {
      "query": "SELECT * FROM processes WHERE on_disk = 0;",
      "interval": 60
    }
  }
}

```
<br>
<img src="https://github.com/user-attachments/assets/780c8ddf-8b2d-4473-bbea-2a4d05b9ab6c" width="434" height="250">
<br>

Save and close the file (in Nano, press CTRL + X, then Y, and Enter).

# Test the Configuration
Verify the JSON Syntax:
<br>
Run
```
sudo osqueryi --config_path /etc/osquery/osquery.conf --verbose
```
This command checks for errors in the config file. If you see errors, fix them before moving on.
<br>
<img src="https://github.com/user-attachments/assets/80c330f8-192a-4cd1-b672-1e35b225d526" width="434" height="350">

Looks good from our results 

# Start OSQuery as a Daemon
Start OSQuery
```
sudo osqueryd --config_path /etc/osquery/osquery.conf
```
To keep OSQuery running in the background, use:

```
sudo systemctl start osqueryd
```
<br>
<img src="https://github.com/user-attachments/assets/cd842684-dc85-4874-be5c-dd64c33fe099" width="574" height="800">

# Time to test running a Query in osqueryi
Steps to Run a Query in osqueryi
<br>
First, ensure that the osqueryd daemon is running. With the following command:
```
sudo osqueryd --config_path /etc/osquery/osquery.conf
```
<br>
Open osqueryi
<br>

```
osqueryi
```
<br>
Run a Query:
<br>
Once you're in the osqueryi shell (you'll see a prompt that looks like osquery>), you can run your query. For example, to check the operating system version, enter:

```
SELECT * FROM os_version
```

<br>
Check the Results: If the query runs successfully, you’ll see output displaying the results of the os_version table, which provides information about your operating system.
<br>

After running your query, you can check the osqueryd.results.log again to see if any entries were added. Use:
```
sudo cat /var/log/osquery/osqueryd.results.log
```
<br>

# Retesting if query works
<img src="https://github.com/user-attachments/assets/ed7a82d5-cb24-4a75-9404-06982ed48141" width="450" height="300">
<br>
<img src="https://github.com/user-attachments/assets/2fd20271-7eee-4d2b-a67a-997a9d21d5f3" width="480" height="300">
<br>
<img src="https://github.com/user-attachments/assets/e9dffc82-93f2-484a-bba8-a596491e3118" width="490" height="700">


# Verifying query logs to Monitor Suspicious Activity 
Use cat to View Results:
```
sudo cat /var/log/osquery/osqueryd.results.log
```
<img src="https://github.com/user-attachments/assets/37a4aed5-00b2-4b9c-8ce3-54c017cdbe26" width="620" height="650">

# Conclusion
This project allowed me to dive into setting up Osquery as a powerful security monitoring tool, giving me hands-on experience with Linux system administration, log management, and crafting custom queries. By configuring Osquery to track essential system information, I demonstrated my ability to use open-source tools for security data collection and analysis.

Through my custom queries, like monitoring user activity, I showcased how I can adapt tools to meet specific security requirements. Although I included sample log data for illustration, I understand what actual log outputs look like and how to interpret them in real-world situations. This project also highlights my troubleshooting skills and confidence in working with command-line tools to meet security goals.

Overall, this experience strengthens my foundation in security monitoring and gives me a solid basis for integrating more advanced and scalable solutions.
