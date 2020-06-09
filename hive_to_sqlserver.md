# 先清空表在抽
```shell
#!/bin/bash
export Azure_DB="jdbc:sqlserver://sql01cndev19909.database.chinacloudapi.cn:1433;database=sqldbeng01cndev19909;"
export user_name=dldeveng
export pwd='deveng@2020'

# city dimension
sqoop eval \
--connect $Azure_DB \
--username $user_name \
--password $pwd \
--query "truncate table [dbo].[rca_user_desc_city_report]"

sqoop export --connect $Azure_DB \
--username $user_name \
--password $pwd \
--table rca_user_desc_city_report \
--hcatalog-database rca_rpt \
--hcatalog-table rca_user_desc_city_report \
--num-mappers 5 \
-- \
--schema dbo \
--direct
```
# sqoop分区抽入sqlserver

```shell
#!/bin/bash
export Azure_DB="jdbc:sqlserver://sql01cndev19909.database.chinacloudapi.cn:1433;database=sqldbeng01cndev19909;"
export user_name=dldeveng
export pwd='deveng@2020'

business_dt=$1

sqoop eval \
--connect $Azure_DB \
--username $user_name \
--password $pwd \
--query "delete from [dbo].[rca_user_desc_report] where rca_report_date=$business_dt"

sqoop export --connect $Azure_DB \
--username $user_name \
--password $pwd \
--table rca_user_desc_report \
--hcatalog-database rca_rpt \
--hcatalog-table rca_user_desc_report \
--hcatalog-partition-keys rca_report_date \
--hcatalog-partition-values $business_dt \
--num-mappers 5 \
-- \
--schema dbo \
--direct
```
