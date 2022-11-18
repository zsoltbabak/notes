# Remap tablespace with imp

```
imp liferay@database fromuser=portal touser=liferay file=dump.dmp indexfile=index.sql

sed -i 's/^REM  \.\.\..*//' index.sql
sed -i 's/^REM  //' index.sql
tr --delete '\n' < index.sql > index2.sql
sed -i 's/ ;/ ;\n/g' index2.sql
sed -i 's/^CONNECT .*//g' index2.sql

sed -i 's/TABLESPACE "WRONG_TABLESPACE"/TABLESPACE "USERS"/g' index2.sql

$ sqlplus liferay@raiffeisen
...
SQL> @index2.sql

imp liferay@database fromuser=portal touser=liferay file=dump.dmp ignore=y

```

```
SQL> set pagesize 2000;
SELECT 'DROP TABLE ' || table_name || ' CASCADE CONSTRAINTS PURGE;' FROM user_tables ORDER BY table_name; 

```
