# Mysql Docker

0. Start docker

0. Run the docker-compose script. The script does the following
	- it downloads a mariadb image and runs the instance
	- sets the root password to the one given
	- creates a database named *camunda_db*
	- creates a user *scott* with the given password

	```
	$ docker-compose up -d
	```

0. Access mariadb instance

	```
	$ mysql -h 127.0.0.1 -u root -p
	Enter password: 
	
	mysql >
	```

0. Download the camunda schema scripts for your version 
	- this poc used v7.8 
	- downloaded from their Nexus repo
	- unzip the zip file.

	```
	$ wget https://app.camunda.com/nexus/content/repositories/camunda-bpm/org/camunda/bpm/distro/camunda-sql-scripts/7.8.0/camunda-sql-scripts-7.8.0.zip
		
	$ unzip camunda-sql-scripts-7.8.0.zip
		
	```
	
0. Run the create scripts for mariadb
	
	```
	$ cd camunda-sql-scripts-7.8.0/create
	$ mysql -h 127.0.0.1 -u camunda -p camunda_db < mariadb_identity_7.8.0.sql > mariadb_identity.out
	Enter password:
		
	$ mysql -h 127.0.0.1 -u camunda -p camunda_db < mariadb_engine_7.8.0.sql > mariadb_engine.out
	Enter password: 
	```
		
0. Verify the schema
	
	```
	$ ○ → mysql -h 127.0.0.1 -u camunda -p camunda_db
	Enter password:
	Reading table information for completion of table and column names
	You can turn off this feature to get a quicker startup with -A
		
	Welcome to the MySQL monitor.  Commands end with ; or \g.
	Your MySQL connection id is 16
	Server version: 5.5.5-10.2.14-MariaDB-10.2.14+maria~jessie mariadb.org binary distribution
		
	Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
		
	Oracle is a registered trademark of Oracle Corporation and/or its
	affiliates. Other names may be trademarks of their respective
	owners.
		
	Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
		
	mysql> show tables;
	+-------------------------+
	| Tables_in_camunda_db    |
	+-------------------------+
	| ACT_GE_BYTEARRAY        |
	| ACT_GE_PROPERTY         |
	| ACT_HI_ACTINST          |
	| ACT_HI_ATTACHMENT       |
	| ACT_HI_BATCH            |
	| ACT_HI_CASEACTINST      |
	| ACT_HI_CASEINST         |
	| ACT_HI_COMMENT          |
	| ACT_HI_DECINST          |
	| ACT_HI_DEC_IN           |
	| ACT_HI_DEC_OUT          |
	| ACT_HI_DETAIL           |
	| ACT_HI_EXT_TASK_LOG     |
	| ACT_HI_IDENTITYLINK     |
	| ACT_HI_INCIDENT         |
	| ACT_HI_JOB_LOG          |
	| ACT_HI_OP_LOG           |
	| ACT_HI_PROCINST         |
	| ACT_HI_TASKINST         |
	| ACT_HI_VARINST          |
	| ACT_ID_GROUP            |
	| ACT_ID_INFO             |
	| ACT_ID_MEMBERSHIP       |
	| ACT_ID_TENANT           |
	| ACT_ID_TENANT_MEMBER    |
	| ACT_ID_USER             |
	| ACT_RE_CASE_DEF         |
	| ACT_RE_DECISION_DEF     |
	| ACT_RE_DECISION_REQ_DEF |
	| ACT_RE_DEPLOYMENT       |
	| ACT_RE_PROCDEF          |
	| ACT_RU_AUTHORIZATION    |
	| ACT_RU_BATCH            |
	| ACT_RU_CASE_EXECUTION   |
	| ACT_RU_CASE_SENTRY_PART |
	| ACT_RU_EVENT_SUBSCR     |
	| ACT_RU_EXECUTION        |
	| ACT_RU_EXT_TASK         |
	| ACT_RU_FILTER           |
	| ACT_RU_IDENTITYLINK     |
	| ACT_RU_INCIDENT         |
	| ACT_RU_JOB              |
	| ACT_RU_JOBDEF           |
	| ACT_RU_METER_LOG        |
	| ACT_RU_TASK             |
	| ACT_RU_VARIABLE         |
	+-------------------------+
	46 rows in set (0.02 sec)
		
	mysql>
	```

0. Camunda BPM
	- update the secrets file with mysql instance details.
	- run Camunda BPM on MiniKube.

0. See logs

	```
	$ docker-compose logs
	```

0. Cleanup
	```
	$ docker-compose stop
	```

0. Prune all images
	```
	$ docker system prune -a
	```
