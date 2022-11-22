# Script To Import Krusty Krab Data

```kusto
.execute database script <|
// Create tables for the data
.create table ['PassiveDns']
(['ip']:string,
['domain']:string)
.create table ['Employees']
(['timestamp']:string,
['name']:string,
['user_agent']:string,
['ip_addr']:string,
['email_addr']:string,
['company_domain']:string,
['username']:string,
['role']:string,
['hostname']:string)
.create table ['OutboundBrowsing']
(['timestamp']:string,
['method']:string,
['src_ip']:string,
['user_agent']:string,
['url']:string)
.create table ['FileCreationEvents']
(['timestamp']:string,
['hostname']:string,
['sha256']:string,
['path']:string,
['filename']:string,
['size']:int)
.create table ['Email']
(['event_time']:string,
['sender']:string,
['reply_to']:string,
['recipient']:string,
['subject']:string,
['accepted']:bool,
['link']:string)
.create table ['AuthenticationEvents']
(['timestamp']:string,
['hostname']:string,
['src_ip']:string,
['user_agent']:string,
['username']:string,
['result']:string)
.create table ['InboundBrowsing']
(['timestamp']:string,
['method']:string,
['src_ip']:string,
['user_agent']:string,
['url']:string)
.create table ['ProcessEvents']
(['timestamp']:string,
['parent_process_name']:string,
['parent_process_hash']:string,
['process_commandline']:string,
['process_name']:string,
['process_hash']:string,
['hostname']:string)
// Import data
.ingest into table Email ('https://github.com/kkneomis/kc7_data/raw/main/krustykrab/Email.csv.gz') with 
(ignoreFirstRecord=true)
.ingest into table Employees ('https://github.com/kkneomis/kc7_data/raw/main/krustykrab/Employees.csv.gz') with 
(ignoreFirstRecord=true)
.ingest into table FileCreationEvents ('https://github.com/kkneomis/kc7_data/raw/main/krustykrab/FileCreationEvents.csv.gz') with 
(ignoreFirstRecord=true)
.ingest into table OutboundBrowsing ('https://github.com/kkneomis/kc7_data/raw/main/krustykrab/OutboundBrowsing.csv.gz') with 
(ignoreFirstRecord=true)
.ingest into table PassiveDns ('https://github.com/kkneomis/kc7_data/raw/main/krustykrab/PassiveDns.csv.gz') with 
(ignoreFirstRecord=true)
.ingest into table AuthenticationEvents ('https://github.com/kkneomis/kc7_data/raw/main/krustykrab/AuthenticationEvents.csv.gz') with 
(ignoreFirstRecord=true)
.ingest into table InboundBrowsing ('https://github.com/kkneomis/kc7_data/raw/main/krustykrab/InboundBrowsing.csv.gz') with 
(ignoreFirstRecord=true)
.ingest into table ProcessEvents ('https://github.com/kkneomis/kc7_data/raw/main/krustykrab/ProcessEvents.csv.gz') with 
(ignoreFirstRecord=true)```
