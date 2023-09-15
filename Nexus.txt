Login as a root user
sudo su -
cd /opt
yum install tar wget -y
wget http://download.sonatype.com/nexus/3/nexus-3.15.2-01-unix.tar.gz
tar -zxvf nexus-3.15.2-01-unix.tar.gz
mv /opt/nexus-3.15.2-01 /opt/nexus

#As a good security practice, Nexus is not advised to run nexus service as a root user, so create a new user called nexus and grant sudo access to manage nexus services as follows.
 
useradd nexus

#Give the sudo access to nexus user

visudo
nexus ALL=(ALL) NOPASSWD: ALL

#Change the owner and group permissions to /opt/nexus and /opt/sonatype-work directories.

chown -R nexus:nexus /opt/nexus
chown -R nexus:nexus /opt/sonatype-work
chmod -R 775 /opt/nexus
chmod -R 775 /opt/sonatype-work

#Open /opt/nexus/bin/nexus.rc file and  uncomment run_as_user parameter and set as nexus user.

vi /opt/nexus/bin/nexus.rc
run_as_user="nexus"

#Create nexus as a service

ln -s /opt/nexus/bin/nexus /etc/init.d/nexus

#Switch as a nexus user and start the nexus service as follows.

sudo su - nexus

#Enable the nexus services
sudo systemctl enable nexus

#Start the nexus service
sudo systemctl start nexus

#Access the Nexus server from Laptop/Desktop browser.
 
http://IPAddess/Hostname:8081/

#Default Credentials
User Name:
Password:

Troubleshooting
---------------------
nexus service is not starting?

a)make sure need to change the ownership and group to /opt/nexus and /opt/sonatype-work directories and permissions (775) for nexus user.
b)make sure you are trying to start nexus service with nexus user.
c)check java is installed or not using java -version command.
d) check the nexus.log file which is availabe in  /opt/sonatype-work/nexus3/log  directory.

Unable to access nexus URL?
-------------------------------------
a)make sure port 8081 is opened in security groups in AWS ec2 instance.

