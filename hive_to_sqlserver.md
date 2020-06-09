# 先清空表在抽
```shell
#!/bin/bash
export Azure_DB="jdbc:sqlserver://abc.database.chinacloudapi.cn:1433;database=abc;"
export user_name=abc
export pwd='abc'

# city dimension
sqoop eval \
--connect $Azure_DB \
--username $user_name \
--password $pwd \
--query "truncate table [dbo].[table_name]"

sqoop export --connect $Azure_DB \
--username $user_name \
--password $pwd \
--table table_name \
--hcatalog-database database \
--hcatalog-table table_name \
--num-mappers 5 \
-- \
--schema dbo \
--direct
```
# sqoop分区抽入sqlserver

```shell
#!/bin/bash
export Azure_DB="jdbc:sqlserver://abc.database.chinacloudapi.cn:1433;database=abc;"
export user_name=abc
export pwd='abc'

business_dt=$1

sqoop eval \
--connect $Azure_DB \
--username $user_name \
--password $pwd \
--query "delete from [dbo].[table_name] where rca_report_date=$business_dt"

sqoop export --connect $Azure_DB \
--username $user_name \
--password $pwd \
--table table_name \
--hcatalog-database database \
--hcatalog-table table_name \
--hcatalog-partition-keys rca_report_date \
--hcatalog-partition-values $business_dt \
--num-mappers 5 \
-- \
--schema dbo \
--direct
```
