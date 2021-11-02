High Availability and Scaling
    Load Balancing Fundamentals
•   LB listens on specific ports (i.e. 443, 80) the LB is referred to as the Listener
•   Clients connect to the LB (listener)
•   The LB connects on your behalf to 1+ targets (servers)
•   Two connections ...listener and backend
•   Client abstracted from individual servers
•   Used for High-Availability, Fault-Tolerance and Scaling
    Application Load Balancer
•   ALB is a "Layer-7" LB - understands HTTP/S
•   capable of inspecting and understand data which passes thru it.  
•   The ALB can take actions bases on the protocols, (i.e. Path, Headers, and Hosts)
•   Scalable and highly-available 
•   Listens on the outside -> sends to Target(s) (Groups)
•   Hourly rate and LCU rate (capacity) (Load Balancer Capacity Units)
•   Cross Zone Load Balancing
•   Target Groups.  A target can be member of multiple groups
•   Rules are path based (i.e. /cat /dog) or host based
•   Support EC2, ECS, EKS, Lambda, HTTPS, HTTP/2 and WebSocket
•   ALB can use (SNI) server name indication for multiple SSL Certs -host based rule
•   Recommended vs CLB (legacy)
    Launch Configurations and Launch Templates
•   Launch Configurations and Launch Templates provide the WHAT to Auto scaling groups.  They define WHAT gets provisioned the AMI, the Instance Type, the networking & security, the key pair to use, the user-data to inject and IAM Role to attach.
•   Key concepts 
•   Allow you to define the configuration of an EC2 instance in advance
•   AMI, Instance type, storage, key pair, networking, security groups, user-data, and IAM role
•   Both are NOT editable - define once.  LT has version
•   LT provide newer features including T2/T3 unlimited, placement groups, capacity reservations, elastic graphics
    Auto Scaling Groups
•   Automatic Scaling and Self-Healing for EC2
•   Uses Launch Templates or Configurations
•   Has a Minimum, Desired and Maximum
•   Provision or Terminate instances to keep at the Desired level (between Min/Max)
•   Scaling Policies - automate based on metrics
•	Manual Scaling - Manually adjust the desired capacity
•	Scheduled Scaling - Time based adjustment
•	Dynamic Scaling
•	Simple - CPU above 50% +1 CPU below 50% -1
•	Stepped Scaling - Bigger +/- based on difference
•	Tracked Tracking - Desired Aggregate CPU = 40% ... ASG handle it
•   Cool down periods to avoid rapid scaling
•   Think about more, smaller instances - granularity
•   Use with ALB's for elasticity - abstraction
•   ASG defines WHEN and WHERE, LT defines WHAT
   Network Load Balancer
•   NLB are layer 4 and understand TCP and UDP
•   Can't understand HTTP/S but are faster 100ms vs 400ms for ALB
•   Millions of requests per second highest performing LB
•   Only AWS LB family can be assigned a static IP address 1 interface with static IP per AZ, can use elastic IPs (whitelisting)
•   Can do SSL pass through.  Anything above TCP is passed.
•   Can load balance no http/s applications - doesn't care about anything above TCP/UDP
             SSL Offload and Session Stickiness
   SSL Bridging
•   Default LB Listener is configured for HTTPS.
•   Connection is terminated on the LB
•   Needs a SSL certificate for the domain name the application uses.
•   AWS does have some level of access to that certificate
•   The LB makes a second back end compute resources
•   LB can see the HTTP traffic (after decryption) , it can take actions based off the contents of the HTTP. Then create new encrypted sessions between it and the EC2 instances.
•   EC2 instances will need matching SSL certificates
•   High volume applications could experience high overhead
  SSL Pass-through
•   The load balancer passes the connection to the back-end instances.  The ELB doesn't decrypt at all.
•   The instances need certificates installed. The ELB doesn't.
•   Network Load balancer which is able to perform this style of architecture.  LB is configured to             listen using TCP
•   AWS never see the certificate it's managed by you.
•   Traffic can't be load balanced based on HTTP.
Offload
•   Listener is configured for HTTPS.  Connections are terminated on LB.
•   The backend connections use HTTP.
•   No certificates on instances.
Connection stickiness
•   Without stickiness connections are distributed across all in-service backend instances.  Unless applications handle the user-state this could cause user logoffs, shopping cart losses. Etc
ELB has the option to enable stickiness.
•   After initial request a cookie that locks the device to a single instance for a duration (enable on a   target group)
•   Common use case is when a user state is stored on an individual instance.
•   Problem with this design is that it can cause uneven loads on backend servers.
•   Ideal situation would be to host the user state or the session on something external like Dynamo DB.  This creates stateless EC2 instances.







    




