•	### BGP
•	Autonomous System (AS) - Routers controlled by on e entity
•	ASN are unique and allocated by IANA (0-65535), 64512-65537 are private
•	BGP operates over tcp/179 it's reliable
•	Not automatic - peering is manually configured
•	BGP is a path-vector protocol it exchanges the best path to a destination between peers ... the path is called the ASPATH


•	AWS Site-to-Site VPN
•	AWS Site-to-Site VPN is a hardware VPN solution which creates a highly available IPSEC VPN between an AWS VPN and external network such as on-premises traditional networks

•	VPNs are quick to setup vs direct connect, don't offer the same high performance, but do encrypt data in transit

•	A logical connection between a VPC and on-premises network encrypted using IPSec, running over the public internet

•	Full HA - if you design and implement correctly
•	Components include:
•	Virtual Private Gateway (VGW)
•	Customer Gateway
o	VPN connection between the VGW and CGW
•	Static VPN
•	Routes for remoted side added to route tables as static routes
•	Networks for remote side statically configured on the VPN connection
•	(no loadbalancing and multi connection failover)
•	Dynamic VPN
•	Border Gateway Protocol
•	BGP is configured on both the customer and AWS side using (ASN).  Networks are exchanged via BGP
•	VPN considerations:
•	Speed Limitation 1.25  Gbps
•	Latency Considerations -  inconsistent, public internet
•	Speed of setup - hours .. all software configuration
•	Can be used as a backup to Direct Connect
•	Can be used with Direct Connect

•	Direct Connect
•	A 1Gbps or 10 Gbps Network Port into AWS
•	..at a DX location (1000-Base-LX or 10GBase-LR)
•	to your (CR) Customer Router (requires VLANS/BGP)
•	or Partner Router (if extending to your location)
•	Direct Connect is AWS's physical private link connecting your business premises to its public and private services.
•	Multiple Virtual Interfaces (VIFS) over one DX
•	Private VIF (VPC) and Public VIF (Public Zone Services)
•	VIFS = one VLAN and One BGP session at the client side
•	Private VIF = One VPC
•	Public VIF = Public Services (NO PUBLIC INTERNET)
•	Low consistent latency and high speeds

•	AWS Transit Gateway
•	The AWS Transit gateway is a network gateway which can be used to significantly simplify networking between VPC's, VPN and Direct Connect.
•	It can be used to peer VPCs in the same account, different account, same or different region and supports transitive routing between networks.
•	Network Transit Hub to connect VPCs to on premises networks
•	Significantly reduceds network complexity
•	Single network object - HA and Scalable
•	Attachments to other netowrk types VPC, Site to Site VPN and Direct Connect Gateway
•	VPC attachments are configured with a subnet in each AZ where service is required
•	Peer with other Transit Gateway Cross-Region Same/Cross-Account
•	TGW can integrate with direct connect gateway using a transit VIF
•	TGW supports multipe route tables allowing complex routing architectures
•	Support transitive routing (no full mesh needed) will orchestrate the routing of traffic between attachments.
•	Can be used to create global networks
•	Share between accounts using AWS RAM
•	Peer with different regions ... same or cross account
•	Less complexity vs w/o TGW

•	### Storage Gateway
•	Storage Gateway is a super flexible hybrid storage appliance.  Its capable of running in 3 modes:
	FILE
	File Storage backed by S3 objects
	Files are stored to share using NFS or SMB
	SMB shares can be integrated with AD for file authorization
	Lifecycle policies can automatically control Storage Classes
	Mapped Directly 1:1 as S3 objects
	TAPE
	Virtual tapes => S3 and Glacier
	pretends to be a iSCSI tape library, changer and drive
	Virtual tape 100 GiB => 5 TiB 
	1 PB total storage across 150 virtual tapes
	Active tapes are stored in S3, archived/exported tapes are stored in VTS in Glacier
	Unlimited VTS (archive) storage
	VOLUME (Stored or Cached). 
	Block storage backed by S3 and EBS snapshots
	Stored Mode (ideal for migration and disater recovery)
	16TB per volume, 32Volumes (MAX)
	512TB total capacity
	Primary data is stored on premises backup data is asynchronously replicated to AWS
	AWS side creates EBS snapshots from backup data.  Can be used to create standard EBS volumes.  Ideal for migrations to AWS
	Cached Mode
	Primary Data is stored in AWS. Data which is accessed frequently is cached locally.  Ideal for extending storage into AWS
	Primary data is stored on a S3-Backed volume (AWS Managed Bucket) snapshots are stored as standard EBS Snapshots
	Volumes are made available via iSCSI for network based servers to access.   Single connection per volume unless servers are clustered.
•	Hybrid Storage Virtual Appliance (On-premise*)
•	Extension of File and Volume Storage into AWS
•	Volume storage backups into ASW
•	Tape Backups into AWS
•	Migration of existing infrastructure into AWS

•	### Snowball
•	Ordered from AWS, Log a job, device deliver (not isntant)
•	Data encryption uses KMS
•	50TB or 80 TB capacity ( 2 diff types of devices)
•	1 Gpbs (RJ45 1GBase-TX) or 10Gbps (LR/SR) network
•	10 TB to 10PB economical range (multiple devices) (versus going over the internet or direct connect)
•	Multiple devices to multiple premises
•	only storage

•	### Snowball Edge
•	Both Storage and Compute
•	Larger capacity vs snowball
•	10 Gbps (RJ45) 10/25(SFP) 45/50/100 Gbps (QSFP+)
•	Storage Optimized (with EC2) - 80TB, 24 vCPU, 32 Gib RAM, (1 TB SSD)
•	Compute Optimized - 100 + 7.68 NVME, 52 vCPU and 208 GiB RAM
•	Compute Optimized   100TB + 7.68 NVME, 52 vCPU and 208 GiB RAM
•	Ideal for remote sites or where dta processing on ingestion is needed

•	### Snowmobile
•	Portable DC within a shipping container on a TRUCK!
•	Special order
•	Ideal for single location when 10 PB+ is required
•	Up to 100PB per snowmobile
•	Not economical for multi-site (unless huge) or sub 10 PB

•	Directory Service
•	The Directory service is a product which provides managed directory service instances within AWS

•	What's a Directory:
•	Stores objects (e.g. Users, Groups, Computers, Servers, File Shares) with a structure (domain/tree)
•	Multiple trees can be grouped into a forest
•	Commonly used in Windows Environments...
•	Sign-in to multiple devices with the same username/password provides centralised management for assets
•	...Microsoft active directory domain services (AD DS)
•	AD DS most popular, open-souce alternatives (SAMBA)

•	AWS Managed  implementation
•	Runs within a VPC
•	To implement HA ... deploy into multiple AZs
•	Some AWS services NEED a directory e.g. Amazon Workspaces
•	Can be isolated or integrated with existing on premise system
•	Or act as a 'proxy' back to on premises

•	Functions in three modes

•	Simple AD - An implementation of Samba 4 (compatibility with basics AD functions) (The default. simple requirements)
•	AWS Managed Microsoft AD - An actual Microsoft AD DS Implementation.  
•	AD Connector which proxies requests back to an on-premises directory.
•	Use AWS services which need a directory #without storing any directory info in the cloud#....proxy to your on-premises directory

•	AWS DataSync
•	AWS DataSync is a product which can orchestrate the movement of large scale data (amounts or files) from on-premises NAS/SAN into AWS or vice-versa
•	Data Transfer service TO and FROM AWS
•	Migrations, Data Processing Transfers, Archival/ Cost Effective Storage or DR/BC
•	...designed wot work at huge scale
•	keeps metadata (e.g. permissions/timestamps)
•	Built in data validation
•	Key Features:
•	Scalable - 10Gbps per agent(~100TB per day)
•	Bandwidth Limiters (avoid link saturation)
•	Incremental and scheduled transfer optons
•	Compression and Encrption
•	Automatic recovery from transit errors
•	AWS service integration S3, EFS, FSx
•	Pay as you use ... per GB cost for data moved

•	DataSync Agent runs on a virtualization platform such as VMware and communicates with the AWS Datasync Endpoint

•	Encryption in transit (TLS)

•	Schedules can be set to ensure the transfer of data occurs during or avoiding specific time periods.  Locations define the SRC or DST for the sync of data TO or FROM AWS e.g S3, EFS, FSx, NFS, SMB

•	TASK - a job within DataSync, defines what is being synced, how quickly, FROM where and TO where

•	Agent - Software used to read or write to on-premise data stores using NFS or SMB

•	Location - every task has two locations FROM and TO.  E.g. Network File System (NFS), Server Message Block (SMB) Amazon EFS, Amazon FSx and Amazon S3

•	FSx for Windows Servers 
•	FSx for Windows Servers provides a native windows file system as a service which can be used within AWS, or from on-premises environments via VPN or Direct Connect  FSx is an advanced shared file system accessible over SMB, and integrates with Active Directory (either managed, or self-hosted).  It provides advanced features such as VSS, Data de-duplication, backups, encryption at rest and forced encryption in transit.

•	Fully mnaaged native windows file servers/shares
•	Designed for integration with windows environments
•	Integrates with Directory Service or Self-Manageed AD
•	Single or Multi-AZ within a VPC
•	on-demand and Scheduled Backups
•	Accessible using VPC, Peering, VPN, Direct Connect
•	Native windows file systems, supports de-duplication (sub file), Distributed File System (DFS), KMS at-rest encryption and enforced encryption in-transit

•	Support voume shadow copies (file level versioning)
•	Performance 8MB/s 2GB/s, 100K IOPS, <1ms latency
•	Key features:
•	VSS - User-Driven Restores
•	Native file system accessible over SMB*
•	Windows permission model*
•	Supports DFS .. scale-out file share structure
•	Managed - no file server admin
•	Integrates with DS and your own directory*



•	FSx for Lustre
•	FSx for Lustre is a managed file system which uses the FSx product designed for high performance computing  It delivers extreme performance for scenarios such as Big Data, Machine Learning and Financial Modeling

•	Managed Lustre - Designed for HPC - LINUX Clients (POSIX)
•	Machime Learning, Big Data, Financial Modeling
•	100's GB/s throughput and sub millisecond latency
•	Deployment Types - Persistent or Scratch
•	Scratch  - Highly optimized for short term no replication and fast (pure performance) short term or temp workloads are ideal
•	Persistent - longer term, HA (in one AZ), self-healing
•	You can backup to S3 with both!! (Manual or Automatic 0-35 day retention)

•	Accessible over VPN or Direct connect
•	S3 bucket lazy load to and from FSx where the processing occur.
•	Metadata stored on a Metadata Targets (MDTs)
•	Burst up to 1300 MB/s per TiB (Credit system)
•	Earn credits when you run below threshold and use when you go above




        




