# OfficeWorksAutomate
Sybase,MySQL stuff to automate

Script1: it is about to create table with colums containing toal free ,used and percentage spae avialble for database  present in differnt servers

#!/bin/ksh

SNAME='server1 serverServer2 server3 server4 serverServ5 serverser6 server7 server8 serverServer9'

SAMPLE_REPORT=sampledbSpaceReport-$(date +%Y%m%d_%H%M%S)
awk 'BEGIN {printf "%s        %s    %s    %s    %s\n","Server_Name", "TOTAL_in_Mb","Free_in_MB","Used_in_MB", "Total_Used_PCT"}' $SAMPLE_REPORT>>$SAMPLE_REPORT
for ser in $SNAME
do
isql -Uusername -S$ser -w9999 -Ppasssowrd  << EOF > /VINOD/samplespace.out
select @@servername,getdate()
go
use SampleDB
go
sp_helpsegment 'default'
go
EOF
 echo -n "$ser" > /VINOD/report_new
 tail -n2 /VINOD/samplespace.out| head -1 >>/VINOD/report_new
 awk '{printf "%-18s %10.2f    %10.2f     %10.2f      %3.2f\n",$1, $3/512, $4/512 , $5/512 ,$5/$3*100}' /VINOD/report_new>> $SAMPLE_REPORT
done
rm /VINOD/report_new
rm /VINOD/samplespace.out





[vinod@server1 VINOD]$ cat sampledbSpaceReport-20190624_125559
Server Name        TOTAL_in_Mb    Free_in_MB    USed_in_MB    total_Used_PCT
server1            1941131.00     114476.47     1826654.53      94.10
serverServer2      1785701.00      65259.39     1720441.61      96.35
server3            1789528.00      61773.76     1727754.24      96.55
server4            1819528.00      83883.48     1735644.52      95.39
serverServ5        1789528.00      57745.03     1731782.97      96.77
serverser6         1881131.00     134961.01     1746169.99      92.83
server7            1822911.00      57599.95     1765311.05      96.84
server8            1792911.00      84930.54     1707980.46      95.26
serverServer9      1879528.00      70113.27     1809414.73      96.27

