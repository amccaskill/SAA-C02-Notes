Elastic Compute Cloud 
EC2 Basics

EC2 provides IaaS - Infrastructure as a Service

Virtualization 101
    The process of running more than one operating system on physical hardware
    
    Normally servers  without virtualization 
        Privileged Mode - only allow the OS kernel to directly interact with the hardware
        User Mode - Applications on top of the OS run in this mode.  Applications need a system call to the kernel inorder to interact with the hardware.  Otherwise it causes an error
     Emulated Virtualization   
        Software Virtualization - Host Operating System / Hypervisor runs in priviledge mode and has full access t the hardware on the server, 
        The guest operating system in each individual VM BELIEVE  they are in control - it is actually only areas of the disk, CPU and memory provided by the hypervisor
         Binary Translation - The calls are intercepted by the hypervisor and translated for the resources.  THIS IS DONE IN SOFTWARE - This means it is slow.

    Para-Virtualization
        Modified operating system that makes calls to the hypervisor instead of the translation done in the hypervisor.  The calls are to the hypervisor instead of the hardware - HyperCalls - 

        Still a set software process design to trick the hardware

    Hardware Assisted Virtualization
        The physical hardware becomes virtualization aware - 
        The CPU contains specific instructions and capabilities so the hypervisor can directly control and configure the support.  
        
        The CPU is aware that it is performing Virtualization
        When the guest operating system (kernel) attempts to run any privledge instructions - They are trap by the CPU, which knows to expect them, so the system as a whole, doesn;t halt. 

        But the instructions can't be executed as is because the guest operating system still thinks that it's running directly on the hardware.  They are redirected to the hypervisor by the hardware.

        The hypervisor handles how these are executed.
            The guest operating systems believe that they have is physical hardware, for examaple, a network card.  But these cards are just logical devices using a driver which actually connect back to a single physical piece of hardware, which sits in the host, the hardware everything is running on.

            Unless you have a physical netowrk card per virtual machine, there's always going to be some level of sottware getting in the way.  When you're performing highly transactional activities, such as network I/O or disk I/O, this really impacts performance, and it consumes a lot of CPU cycles on the host.

    SR-IOV
        Single route I/O virtualization, Now I could talk about this process for hours about exactly what it does and how it works, because it's a very complex and feature rich set of standards, But at a very high level, it allows a network card or any other add-on to present itself, not as one single card, but almost as several mini cards.
        Because this is supported in hardware, these are fully unique cards as far as the hardware is concered.  And these are presented to the guest operationg system as real cards dedicated for its use
        No translation by the hypervisor
        The SR-IOV handles the process end to end.  It makes sure that when the guest operating systems use their logical mini netowrk cards, that they have physical access to the physical network connection when required
        in EC2 - This is Enhanced Networking
        consistanly low lantency and low CPU utilization
        NITRO is the AWS hypervisor

        



















Building Wordpress manually

# DBName=database name for wordpress
# DBUser=mariadb user for wordpress
# DBPassword=password for the mariadb user for wordpress
# DBRootPassword = root password for mariadb

# STEP 1 - Configure Authentication Variables which are used below
DBName='a4lwordpress'
DBUser='a4lwordpress'
DBPassword='REPLACEME'
DBRootPassword='REPLACEME'

# STEP 2 - Install system software - including Web and DB
sudo yum install -y mariadb-server httpd wget
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2


# STEP 3 - Web and DB Servers Online - and set to startup

sudo systemctl enable httpd
sudo systemctl enable mariadb
sudo systemctl start httpd
sudo systemctl start mariadb


# STEP 4 - Set Mariadb Root Password
mysqladmin -u root password $DBRootPassword


# STEP 5 - Install Wordpress
sudo wget http://wordpress.org/latest.tar.gz -P /var/www/html
cd /var/www/html
sudo tar -zxvf latest.tar.gz
sudo cp -rvf wordpress/* .
sudo rm -R wordpress
sudo rm latest.tar.gz


# STEP 6 - Configure Wordpress

sudo cp ./wp-config-sample.php ./wp-config.php
sudo sed -i "s/'database_name_here'/'$DBName'/g" wp-config.php
sudo sed -i "s/'username_here'/'$DBUser'/g" wp-config.php
sudo sed -i "s/'password_here'/'$DBPassword'/g" wp-config.php   
sudo chown apache:apache * -R


# STEP 7 Create Wordpress DB

echo "CREATE DATABASE $DBName;" >> /tmp/db.setup
echo "CREATE USER '$DBUser'@'localhost' IDENTIFIED BY '$DBPassword';" >> /tmp/db.setup
echo "GRANT ALL ON $DBName.* TO '$DBUser'@'localhost';" >> /tmp/db.setup
echo "FLUSH PRIVILEGES;" >> /tmp/db.setup
mysql -u root --password=$DBRootPassword < /tmp/db.setup
sudo rm /tmp/db.setup


# STEP 8 - Browse to http://your_instance_public_ipv4_ip
