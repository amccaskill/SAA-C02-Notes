High Availability and Scaling
    Load Balancing Fundamentals
        1.  LB listens on specific ports (ie 443, 80) the LB is refered to as the Listener
        2.  Clients connect to the LB (listener)
        3.  The LB connects on your behalf to 1+ targerts (servers)
        4.  2 connections ..  listener and backend
        5.  Client abstracted from individual servers
        6. Used for High-Availability, Fault-Tolerance and Scaling
    Application Load Balancer
        1.  ALB is a "Layer-7" LB -  understands HTTP/S
        2.  capabile of inspecting and understand data which passes thru it.  
        3.  The ALB can take actions bases on the protocols, (ie Path, Headers, and Hosts)
        4.  Scalable and highly-available 
        5.  Listens on the outside -> sends to Target(s) (Groups)
        6. Hourly rate and LCU rate (capacity) (Load Balancer Capacity Units)
        7.  Cross Zone Load Balancing
        8.  Target Groups.  A target can  be member of multiple groups
        9.  Rules are path based (ie /cat /dog) or host bsed
        10.  Support EC2, ECS, EKS, Lambda, HTTPS, HTTP/2 and Websockets
        11.  ALB can use (SNI) server name indication for multiple SSL Certs -host based rule
        12.  Recommended vs CLB (legacy)
    Launch Configurations and Launch Templates
        1.  Launch Configurations and Launch Templates provide the WHAT to Auto scaling groups.  They define WHAT gets provisioned The AMI, the Instance Type, the networking & security, the key pair to use, the userdata to inject and IAM Role to attach.
        2.  Key concepts 
            1.  Allow yo uto define the configuration of an EC2 instance in advance
            2.  AMI, Instance type, storage, key pair, networking, security groups, userdata, and IAM role
            3.  Both are NOT editable -  define once.  LT has version
            4.  LT provide newer features including T2/T3 unlimited, placement groups, capacity reservations, elastic graphics
    Auto Scaling Groups
        1. Automatic Scaling and Self-Healing for EC2
        2.  Uses Launch Templates or Configurations
        3.  Has a Minimum, Desired and Maximum
        4. Provision or Terminate instances to keep at the Desired level (between Min/Max)
        5. Scaling Policies - automate based on metrics
            1.  Manual Scaling - Manually adjust the desired capacity
            2.  Scheduled Scaling - Time based adjustment
            3. Dynamic Scaling
                1.  Simple - CPU above 50% +1 CPU below 50% -1
                2. Stepped Scaling - Bigger +/- based on difference
                3.  Tracked Tracking - Desired Aggregate CPU = 40% ... ASG handle it
        6.  Cool down periods to avoid rapid scaling
        7.  Think about more, smaller instances - granularity
        8. Use with ALB's for elasticity - abstration
        9. ASG defines WHEN and WHERE, LT defines WHAT
    Network Load Balancer
        1.  NLB are layer 4 and understand TCP and UDP
        2.  Can't understand HTTP/S but are faster 100ms vs 400ms for ALB
        3.  Millions of request per second highest performing LB
        4. Only AWS LB family can be assigned a static IP address 1 interface with static IP per AZ, can use elastic IPs (whitelisting)
        5.  Can do SSL pass through.  Anything above TCP is passed.
        6. Can load balance no http/s applications - doesn't care about anything above TCP/UDP
    SSL Offload and Session Stickiness
        1. SSL Bridging
            1. Default LB Listener is configured for HTTPS.
            2.  Connection is terminated on the LB
            3.  Needs a SSL certificate for the domain name the application uses.
            4. AWS does have some level of access to that certificate
            5.  The LB makes a second back end compute resources
            6.  LB can see the HTTP traffic (after decryption) , it can take actions based off the contents of the HTTP .  Then create new encrypted sessions between it and the EC2 instances.
            7.  EC2 instances will need matching SSL certificates
            8.  High volume applications could experience high overhead
        2.  SSL Pass-through
            1.  The load balancer passes the connection to the back end instances.  The ELB doesn't decrypt at all.
            2.  The instances need certificates installed. The ELB doesn't.
            3.  Network Load balancer which is able to perform this style of architecture.  LB is configured to listen using TCP
            4.  AWS never see the certificate it's managed by you.
            5. Traffic can't be load balanced based on HTTP.

        3. Offload
            1.  Listener is configured for HTTPS.  Connections are terminted on LB.
            2.  The backend connections use HTTP.
            3.  No certificates on instances.
    Connection stickiness
        1. Without stickiness connections are distributed across all in-service backend instances.  Unless applications handles the user state this could cause user logoffs, shopping cart losses..etc
        2.  ELB has the option to enable stickiness.
            1.  After initial request a cookie that locks the device to a single instance for a duration (enable on a target group)
            2.  Common use case is when a user state is stored on an individual instance.
            3.  Problem with this design is that it can cause uneven loads on backend servers.
            4.  Ideal situation would be to host the user state or the session on something external like Dynamo DB.  This creates stateless EC2 instances.
    




