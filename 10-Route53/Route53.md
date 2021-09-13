
R53 Hosted Zones (Route 53)
1. A Route 53 Hosted Zone is a DNS DB for a domain 
2. Globally Resilient
3. Created with domain registration vis R53- Can be created separately
4. Hosted DNS records ( A, AAAA, MX, NS, TXT)
5. Hosted Zones are what the DNS system references Authoritative for a domain 
6. R53 Public Hosted Zones
    1. DNS Database (zone file) hosted by R53 (Public Name Servers)
    2. Accessible form the public internet and VPCs
    3. Hosted on “4’ R53 NAME SERVERS (NS) specific for the zone
    4. Uses “NS records” to point at these NS (connect to global DNS)
    5. Resource Records (RR) created within the Hosted Zone
    6. Externally registered domains can point at R53 Public Zone

Walking the (DNS)Tree:
1. Internet Service Provider (ISP) Resolver———>.ORG NS Records
    1. .ORG TLD Server——> NS Records
        1. Public R53 Hosted Zone (4 R53 NS Per Hosted Zone)
            1. Resource Records inside Hosted Zone 
                1. (WWW A 1.2.3.4, MX 10 some server, MX 20 some server, TXT “verify something” )


R53 Private Hosted Zones

1. Public Hosted Zone isn’t public
2. Associated with VPC
3. Only accessible in those VPCs
4. Using different accounts is supported via CLI/API 
5. Split-view (overlapping public and private) for PUBLIC and INTERNAL use with the same zone name

* Private Host Zones are for providing (sensitive) records using DNS that need to be accessible for a VPC
    * Records ONLY accessible for services running inside a VPC that is associated with the PRIVATE HOSTED ZONE

* Split-view (overlapping public and private) for PUBLIC and INTERNAL use with the same zone name.
    * Inside VPC associated with the private hosted zone all resource records can be accessed.  A common architecture is to make the private zones superset containing more sensitive records
    * Resource records in the private hosted zone are not accessible from the public internet


R53 CNAME vs ALIAS
1. “A” maps a NAME to an IP address
2. …catagram => 1.3.3.7
3. CNAME maps a NAME to another NAME
4. …www.catagram.io => catagram.io
5.  CNAME is INVALID for NAKED/APEX 
6. Many AWS services use a DNS Name
7. ALIAS records map a NAME to an AWS resource
8. ALIAS records can be used for both naked/apex and normal records
9. ALIAS for non apex/naked - functions like a CNAME
10. There is no charge for ALIAS requests pointing at AWS resources
11.  For AWS Services - default to picking ALIAS
12. ALIAS is actually a sub “TYPE” of record (CNAME or A record)
13. API Gateway, CloudFront, Elastic Beanstalk, ELB, Global Accelerator and S3


Route53 Simple Routing

* Simple Routing supports 1 record per name (WWW)
* Each Record can have multiple values
* All values are return in a random order
* Client choose and uses 1 value
* Not Flexible 
* Simple routing doesn’t support health checks -  all values are returned for a record when queried


R53 Health Checks
* Are separate from, but are used by records
* Health checkers located globally
* Every 30s (every 10s costs extra)
* TCP, HTTP/HTTPS with String Matching
* Status code 200 or 300 range after 2 seconds of connecting
* Amazon Route 53 health checks monitor the health and performance of your web applications, web servers, and other resources
* Either Healthy or UnHealthy
* Types of Health Check
    * Endpoint, CloudWatch Alarm, Checks of Checks (Calculated)
* Each health check that you create can monitor one of the following:
    * The health of a specified resource, such as a web server
    * The status of other health checks
    * The status of an Amazon CloudWatch alarm


Route53 Failover Routing

1. If the target of the health check is ‘Healthy’ the Primary record is used
2. If the target of the health check is Unhealthy..any queries return the secondary record of the same name
3. A common architecture is to use failover / maintenance page for a service (e.g EC2 / S3)
4. Failover routing lets you route traffic to a resource is healthy or to a different resource when the first resource is unhealthy

Route53 Multi value Routing
1. Mixture between simple and failover taking the benefits of each and merging them into one routing policy.
2. Its aim is to use DNS to improve the availability of an application.  It is not a replacement for load balancing.
3. Use when you have many resources that can service request and you want them health checked.

4. Multivalue answer routing lets you configure Amazon Route 53 to return multiple values, such as IP addresses for your web servers, in response to DNS queries.  You can specify multiple values for almost any record, but Multivalue answer routing also lets you check the health of each resource, so Route 53 returns only values for healthy resources

Route53 Weighted Routing
1. Simple load balancing or testing new software versions
2. Each record in the “Hosted Zone” has a Record Weight
3. Each record is returned based on its record weight vs total weight
4. If a chosen record is unhealthy, the process of selection is repeated until a healthy record is chose

Route53 Latency-Based Routing
1. Use latency-based routing if your application is hosted in multiple AWS regions, you can improve performance for your users by serving their request from the AWS region that provides the lowest latency.
2. Use latency based routing when optimizing for performance and user experience
3. AWS maintains a database of latency between the users general location and the regions tagged in records
4. Latency Based routing supports one record with the same name in each AWS region

Route53 Geolocation Routing
1. Geolocation routing lets you choose the resources that serve your traffic based on the geographic location of your users, meaning the location that DNS queries originate from.
2. With Geolocation records are tagged with location.  Either US, State, “country”, “continent”, or “default”
3. An IP check verifies the location of the user (normally the resolver)
4. R53 checks for records in the:
    1. State
    2. Country
    3. The continent
    4. Default- it returns the most specific record or “no answer”
5. Geolocation doesn’t return “closest” records, only RELEVANT (location) records
6.  Can be used for regional restrictions, language specific content or load balancing across regional endpoints.

Route53 Geo-proximity Routing
1. May seem similar to latency based but is base on lowest distance to record.
2. Define Rules or provide lat and long if external request (non-AWS) resource
3. Records can be tagged with an AWS Region or lat and long coordinates
4. “+” or “-“ bias can be added to rules.  “+” increases a region size and decreases neighboring regions
5. Bias allow flexible rules for extending or decreasing a coverage area of the globe.
    1. Use cases where the you want the request to stay in the country boundary and ignore distance.

Route53 Interoperability
1. When you register a domain with AWS you actually do two things:
    1. Domain register and Domain Hosting
    2. R53 can do BOTH, or either Domain Register or Domain Hosting
        1. Register Domain
            1. R53 accepts your money (domain registration fee)
            2. R53 allocates 4 Name Server (NS) (domain hosting)
            3. R53 Creates a zone file (domain hosting) on the above NS
            4. Domain Registrar
                1. R53 communicates with the registry of the TLD (Domain Registrar)
                2. Sets the NS records for the domain to point at the 4 NS above