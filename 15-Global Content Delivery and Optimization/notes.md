	
* CloudFront	
* CloudFront is AWS's Content Delivery Network (CDN) product.	
* CloudFront is a global object cache (CDN)	
* Content is cached in locations close to customer	
* Lower latency and higher throughput	
* It can handle static and dynamic content	
* CloudFront Terms	
* Origin - The source location of your content	
* Distribution - the 'Configuration' unit of CloudFront	
* Edge Location -  Local infrastructure which hosts a cache of your data	
* Regional Edge Cache - Larger version of an edge location.  Provides another layer of caching.	
* No write caching.  Any uploads go directly to origin, CDN for download only.	
* Caching Optimisation	
* Query String Parameters (everything has to match) could lead to removing the benefits of CDN	
* Getting the most out of cloudfront requires you to correctly configure what is used for caching and what is not, and what is forwarded to the origin and what isn't.	
* Used for Globally static and or dynamic HTTP/S content	
* OIA and Bucket policies are used to ensure only CloudFront can access S3 Buckets.	
* AWS Certificate Manager (ACM)	
* The AWS certificate Manage is a service which allows the creation, management and renewal of certificates. It allows deployment of certificates onto supported AWS services such as CloudFront and ALB.	
* Create, renew and deploy certificates with ACM	
* Supported AWS services ONLY (e.g. CloudFront and ALBs..NOT EC2)	
* Certificate created for DNS name with the ACM and deployed onto supported services.  The edge locations also get the certificate.	
* Edge locations are accessed using HTTPS and DNS name on the certificate	
* Edge locations communicate with the Origin using same protocol.  If HTTPs is used the Origin must have valid trusted certificates.  (no self signed certs)	
* ACM must be created in the region in which the service that uses it.	
* When using CloudFront ACM must be created in us-east-1 region. 	
* Securing CF and S3 using OAI	
* Origin Access Identities are a feature where virtual identities can be created, associated with a CloudFront Distribution and deployed to edge locations.	
* Access to an s3 bucket can be controlled by using these OAI's - allowing access from an OAI, and using an implicit DENY for everything else.	
* They are generally used to ensure no direct access to S3 objects is allowed when using private CF Distributions.	
* Best pratice is to use one OAI per distribution	
* Global Accelerator	
* Anycast IP's allow a single IP to be in multiple locations.  Routing moves traffic to closest location	
* Traffic initially uses the public internet and enters a Global Accelerator edge location	
* From the edge, data transits globally across the AWS global backbone network.  Less hops, directly under AWS control, significantly better performance.	
* AWS Global Accelerator is designed to improve global network performance by offering entry point onto the global AWS transit network as close to customers as possible using ANycast IP addresses	
* Moves the AWS network closer to customers	
* Connections enter at edge ... using anycasts IPs	
* Transit over AWS backbone to 1+ locations	
* Can be used for NON HTTP/S----used for TCP/UDP*** Difference from CloudFront***	