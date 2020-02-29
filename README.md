# AlwaysOn Performance Dashboard

The AlwaysOn Performance Dashboard is a collection of reports that you can use on SSMS to get performance metrics about the SQL Server machine and AlwaysOn Availabiltiy Group. The principals performance counters collected from primary and secondary replicas are:


<Primário>
SQL Server: Databases
	• Log Bytes Flushed/sec
	• Log Flush Write time (ms)
	• Transaction/sec	
SQL Server: Availability Replica
	• Bytes Sent to Replica/sec
	• Bytes Sent to Transport/sec ***
SQL Server: Database Replica
	• Transaction Delay
	• Mirrored Write Transactions/sec
Logical Disk:
	• Avg. Disk sec/Read
	• Avg. Disk sec/Writes
	• Disk Transfer/sec

<Secundário>
SQL Server: Databases
	• Log Bytes Flushed/sec
	• Log Flush Write time (ms)
	• Transaction/sec
SQL Server: Availability Replica
	• Bytes Received from Replica/sec
SQL Server: Database Replica
	• Log Bytes Received/sec
	• Redone Bytes/sec
	• Recovery Queue
	• Log Send Queue
Logical Disk:
	• Avg. Disk sec/Read
	• Avg. Disk sec/Writes
	• Disk Transfer/sec

The setup of the AlwaysOn Performance Dashboard reports consists of the following three phases:

1. Setup.sql 

This T-SQL script creates the schema (rpt), objects and SQL Agent Jobs required for performance data collection. The objects are created on MSDB database and this script need to be ran on all the sql instance (replicas) which needs to be monitored. You can also use this dashboard to monitor CPU, DISK and RAM on SQL Server machine with no Availability Group configured, but in this case I recommend you to take a look at the article https://docs.microsoft.com/en-us/archive/blogs/sql_server_team/sql-server-performance-baselining-reports-unleashed-for-enterprise-monitoring

2. Data Collection Steps for each SQL Instance to Monitor

Open an SSMS connection to the PRIMARY and SECONDARY replicas you want to monitor

Run the setup.sql script on each replica

Check SQL Agent JOBs to see if the new jobs AlwaysOn: PerfDashboard Collection Counter and AlwaysOn: PerfDashboard Purge) are running successfully.

Copy the reports (.rdl files) to a file share (accessible by all replicas) or to a local disk of one of the replicas

On SSMS, open the AlwayOn Dashboard Report (AlwaysOn Performance Dashboard.rdl) using the option Reports\Custom Reports... on the instance level context menu

3. To remove the dashboard

Use this step just if you wan to remove the AlwaysOn Performance Dashboard. On this case, run the script "drop_all_objects" on all replicas to generate a list of all objects used by the AlwaysOn Performance Dashboard. Then execute the output to drop all the objects and jobs.


DISCLAIMER: The sample scripts and dashboard are provided AS IS without warranty or support of any kind. The entire risk arising out of the use or performance of the sample scripts and documentation remains with you.

Change Notes:
--- Replease of v2

