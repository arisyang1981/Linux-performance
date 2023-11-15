# Linux-performance

Trace the resource usage for indiviual process.  
# 1 pidstat  
Documentation: https://man7.org/linux/man-pages/man1/pidstat.1.html  
Scripts:  
```
cat <<+ > test.sh
#!/bin/bash 

test()
{
while true
do 
echo "test outout pf pidstat................................"|base64 >> /dev/null 
done 
}

test 
+
chmod u+x test.sh
```
# Example 1, display the resource usage of disk/memory/cpu for a specific pid in one line.  
Run test.th scripts:  
./test.sh   
Get pid:  
ps -ef|grep -i test.sh  
Trace usages:  
pidstat -h -d -r -u 1 -p ${PID}  
Usages:  
Linux 5.10.186-179.751.amzn2.x86_64 (ip-10-0-1-91.ec2.internal)         11/15/23        _x86_64_        (2 CPU)  
       Time   UID       PID    %usr %system  %guest    %CPU   CPU  minflt/s  majflt/s     VSZ    RSS   %MEM   kB_rd/s   kB_wr/s kB_ccwr/s  Command  
 1700069292  1000     15689    6.00   18.00    0.00   24.00     1  33927.00      0.00  121712   2752   0.07      0.00      0.00      0.00  test.sh  
'-p ${PID}' specifies pid.  
'-h' display all usages in one line.  
'-d' represents IO.  
'-r' represents memory.  
'-u' represents cpu.  
'1' represents print out results every 1 second. 
# Example 2, display the resource usage of disk/memory/cpu for a specific program in one line.  
pidstat -h -d -r -u 1 -C test.sh  
Linux 5.10.186-179.751.amzn2.x86_64 (ip-10-0-1-91.ec2.internal)         11/15/23        _x86_64_        (2 CPU)  
       Time   UID       PID    %usr %system  %guest    %CPU   CPU  minflt/s  majflt/s     VSZ    RSS   %MEM   kB_rd/s   kB_wr/s kB_ccwr/s  Command  
 1700069440  1000     15689    4.95   19.80    0.00   24.75     1  32392.08      0.00  121712   2752   0.07      0.00      0.00      0.00  test.sh  
# Example3, display the resource usage of disk/memory/cpu for a specific program which is running along with pidstat in one line.
Once test.sh finishes, pidstat will finish at the same time.
 pidstat -h -d -r -u 1 -e test.sh  
 '-e' doesn't work on some platforms for example, AWS Linux2, but works on Rocky9, use 'pidstat --help' to verify if the version supports '-e'.  


