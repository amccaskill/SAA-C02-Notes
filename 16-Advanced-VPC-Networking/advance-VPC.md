•	VPC Flow Logs
•	Capture packet metadata..Not packet contents
•	Applied to a VPC -All interfaces in that VPC
•	Subnet - interfaces in that subnet
•	interface directly
•	VPC Flow Logs are not realtime
•	Destination can be S3 or CloudWatch Logs - Flow Logs can be stored on S3 or CloudWatch Logs
•	VPC FLow logs is a feature allowing the monitoring of traffic flow to and from interfaces within a VPC
•	VPC Flow logs can be added at a VPC, Subnet or Interface level.
•	Flow Logs DON'T monitor packet contents ... that requires a packet sniffer.
•	VPC flow log
•	<srcaddr>, <destination>, <srcport>, <dstport>, <protocol>, <action>
•	If you have the same flow and src to dsc accept and return traffic denied.  Possible NACL blocking traffic
•	Egress-Only Internet Gateway
•	With IPv4 addresses are private or public
•	NAT allows private IPs to access public networks
•	...without allowing exernally initiated connectins (IN)
•	With IPv6 all IPs are public
•	Internet Gateway (IPv6) allow all IPS IN and OUT
•	Egress-Only is outbound-only for IPv6
•	Egress-Only Gateway is HA by default across all AZs in the region - scales as required
•	Default IPv6 Route ::/0 added to RT with eigw-id as target
•	Egress-Only internet gateways allow outbound (and response) only access to the public AWS services and Public Internet for IPv6 enabled instances or other VPC based services.
•	Outgoing traffic and it's responses are allowed.  INBOUND denied.
•	Gateway Endpoints
•	Provide private access to S3 and Dynamo DB
•	Prefix list added to route table pointing to GateWay Endpoint
•	Highly Available (HA) across all AZ in a region by default
•	Endpoint policy is used to control what it can access
•	Regional....can't access cross region services
•	Prevent Leaky Buckets S3 Buckets can be set to private only by allowing access ONLY from a gateway endpoint
•	Gateway endpoints are a type of VPC endpoint which allow access to S3 and DynamoDB without using public addressing.
•	Gateway Endponts are not accessible outside the VPC




•	Interface Endpoints
•	Interface endpoints are used to allow private IP addressing to access public AWS services.
•	S3 and DynamoDB are handled by gateway endpoints - other supported services are handled by interface endpoints.
•	Unlike gateway endpoints - interface endpoints are not highly available by default - they are normal VPC network interfaces and should be placed 1 per AZ to ensure full HA.
•	For HA add one endpoint to sone subnet per AZ used in the VPC
•	Network access controlled via Security Groups
•	Endpoint Policies restrict what can be done with the endpoint
•	TCP and IPv4 ONLY
•	Uses Private Link
•	Endpoint provides a NEW service endpoint DNS
•	Endpoint Regional DNS
•	Endopoint Zonal DNS
•	Applications can optionally use these, or
•	PrivateDNS overrides the default DNS for services

•	VPC Peering
•	Direct encryption network link between two VPCs (no more than TWO)
•	Works same/cross-region and same/cross-account
•	(optional) Public Hostnames resolve to private IPs
•	VPC peering is a software define and logical networking connection between two VPC's
•	They can be created between VPCs in the same or different accounts and the same or different regions.
•	Same region SG's can reference peer SGs
•	VPC Peering does NOT support transitive peering
•	Routing Configuration is needed, SGs and NACLs can filter




