LINUX

``` 10 Linux commands that we use in daily basis? ```
ls: with ls, I pass the parameters like -a -l -ltr
cd: to switch the directories
cat, less, tail: to read the logs
grep: to fetch the particular patterns
top, htop, ps – to check the processers and resource utilization of the process
df: disc utilization, df-h: human readable format, du-sh: for a particular directory resource utilization
systemctl: to restart the services pr check the services
chmod/chown: for changing permission and right people having the permission
scp/rsync: to copy the files from my local linux machine to cloud instance
find: to locate particular file in the directory 

	You lost a PEM file, how could you connect to the cloud instance?
PEM file holds the private key for the cloud instance. And private key to the instances
For ssh we need either password or private key
ssh-keygen -t rsa generate the public and private keys, copy the public key in authorized keys folder and copy the private key to local and change the permission to chmod 400 and try connecting ssh -i <private key> user@instanceip

	/var is almost 90% full. What will you do?
/var is the place where the services write the log files like apache
Login to instance—
cd /var      
cd log
sudo du -sh * -- gives space occupied by each folder
 

	In Linux virtual machine production is slow due to high CPU utilization?
Login to instance
Check top or htop which is installed in the machine we will get list of resources running  with PID
Take the PID and execute kill, nice or renice commands
Kill—to kill the process, nice or renice to prioritize the process in cpu so that it will take less priority in cpu so that it can give that to other resources
  

	Application deployed in Nginx returns an error of connection refused. How to fix it?
Error 502 : a request sent from Nginx, but the application is not able to communicate back to Nginx.
Error 404: then it should be issued with location and configuration of the application context path.
For this the connection is refused check first if the Nginx is running or not. To check 
systemctl status nginx
here checking for docker service
 
 After the nginix is running still the connection is refused. Then we will check the firewall. If it is a cloud instance, we will check the security group followed by inbound traffic rules.

	SSH plan instance phase.
1.	Check if the user is using the right IP address and the correct PEM file.
2.	Check the permissions of the pem file with chmod.
3.	Check any firewall on the virtual machine. (disable)
4.	Security group on the cloud instance – make sure port 22 is opened for inbound traffic.
5.	Sshd or ssh process might have stopped
 

	Find and list log files that are older than 7 days in /var/log directory
We will use the `find` command to find the logs. After the `find` command, we will go to that particular directory so that type we are defining whether it was a file or directory. Here we are using the file name, so -type f. After that -name, we need to provide the name of the files we need. Since we don't know the name, we are giving *.log as we are checking the log files. And also we need the older than 7 days files. So we will be giving -mtime + 7
sudo find /var/log -type f -name '*.log' -mtime +7
 
So we want to list all the details of the files that are older than 7 days. We will be using           -exec that is used with find, whatever the output of the find command that will be replace by  the {} here we are using ls -ltr  we can use rm -rf, mv , gzip to zip the files like wise..
sudo find /var/log/ -type f -name ‘*.log’ -mtime +7 -exec ls -ltr {} \;
 







	 How to find and remove log files that are older than 7 days?
sudo find /var/log -type f -name “*.log” -mtime +7 -exec rm  {} \;
 

	Log Rotation
Scenario: “your application writes the logs in /var/ log/my app.
Compress the logs that are older than 7 days.
Delete the logs that are older than 30 days.
Automate this via a cron job daily”
1-- verify first that the directory exists or not
2-- use the Find command to check compressed logs that are older than 7 days. We have a checkpoint here. It should not include the logs that are older than 30 days.
3-- again use the `find` command for deleting the logs older than 30 days.
4-- for record purposes, like on this particular data log, whether the log rotation has been successful or not will be kept a record.
 



#!/bin/bash
#Declare Variables
 LOG_DIR = /var/log
 LOG_ROT = /var/log/logrotation.log
 if [ ! d "$LOG_DIR" ] then
         echo " [$date] ERROR:Log Directory $LOG_DIR does not exist!" >> "$LOG_ROT"
         exit 1
find "$LOG_DIR"  -type f -name "*.log" -mtime +7 ! -mtime -30 ! -name "*.gz" -exec gzip {} \; -exec echo"[$date] sucessfully compressed: {}" >> "$LOG_ROT"
{} \;
find "$LOG_DIR" -type f -name "*.gz" -mtime +30 -exec rm -f {} \; -exec echo " [$date] succesfully deleted: {} " >> "$LOG_ROT" {} \;
echo " [$date] log roatation is sucessfull " >> "$LOG_ROT"

	Bulk user creation using CSV in a virtual machine with a shell script.
1-- we will check first if the file exists or not.
2-- the CSV file data consists only from the line two. So we'll be using the tail command.
3-- as there is no set of numbers or the users defined, we will be going with the while loop.
4-- we are not using the PASSWD because it will display the current password of the user. So we can't do that in the terminal when executing the script. So we will be using CHPASSWD.
#!/bin/bash
FILE="users.csv"
tail -n +2 "$FILE" | while read line
do
        username=$(echo $line | cut -d ',' -f1)
        password=$(echo $line | cut -d',' -f2)
        useradd "$username"
        echo "$username:$password | chpasswd"
        echo "created the user:$username"
done

	Service Health Check Monitor Script Using bash
services (“nginx “ “ssh” “docker”)
Interviewer has provided this and told you to check whether the service is running or not. If not, restart the service.
We can see the services are given in a string format. Change the string format to our desired output. We'll be using ${services[@]}
 
 
#!/bin/bash
services=("nginx" "ssh" "docker")
for service in "${services[@]}"; do
        if systemctl is-active --quiet "$service"; then
                echo "$service service is Running"
        else
                echo "$service is Stopped"
                sudo systemctl start "$service"
                if systemctl is-active --quiet "$service"; then
                        echo "$service restarting sucessfully"
                else
                        echo "failed to restart"
                fi
        fi
done
 

	How to find and delete files over 100 MB?
find /var/lib -type f -size +100M -exec rm -i {}  \;
We will be using the `find` command after that. `find` checks the directory for the type and type which type of the file or directory. Here we need to use the size (like the disk space) so that we'll be using the `-size` parameter we are taking the files over 100 MB, so we're using +100MB after that. We'll be using the -exec where all the find related things will be going to a {} and will be executed before the remove command.
	Get the list of users that are logged in today?
We have a command in Linux to check the people who log into the virtual machine that is “last -F” 
last -F | grep “$(date ‘+%b %e’)” 
After the last command, we'll be using grep to search. In the grep, we need the date. To fetch today's date, we'll be using a command inside a command, “$(date, ‘+%b % e’)”
awk – will print the first column second column any column we want
awk ‘{print $1}’

	If the website  is not loading, how will you investigate?
Website is not loading and port is open.
Any website, if it is running in virtual machines, it is serving a static content then it should be running with the help of a web servers like nginx, apache 
1--The webservers are hosting the static content to check whether webservers are running or not use the command systemctl status nginx
2--We we need to check the logs of the web server. For suppose if it is Nginx, we will be going to /var/log/nginx/error.log
3-- HTML file is in the right location or not (index.html) that is mentioned in nginx.conf file
Default folder for this is /var/www/html

	Using the `sed` command, how to remove the first and last lines of a file?
To remove the first and last line of a file, the sed command is used.
sed ‘1d;$d’
If she is not to print on the terminal, then
sed -I ‘1d;$d’
$d – represents last line of the file.
 

