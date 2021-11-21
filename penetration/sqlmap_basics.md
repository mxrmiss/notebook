# Sqlmap 基础

### Format

```
sqlmap -u "required_url" parameter1, parameter2, parameter3 ...
```

### Basic parameters

```
-dbs		//all databases
-tables		//all tables
-columns	//all columns
-dump		//all items of columns
-D [DBNAME]		//specified database
-T [TBLNAME]	//specified table
-C [COLNAME]	//specified column
-random-agent	//construct random users
-proxy [PROXY]://[url]:[port] -threads 10	//proxy(customize the thread)
–dbms=DBMS			//specified DBMS
-p	[TESTPARAMETER]		//specified parameters of a test
–level=[LEVEL]		//level of execution (1-5)
–risk=[RISK]		//risk of execution (0-3)
```

