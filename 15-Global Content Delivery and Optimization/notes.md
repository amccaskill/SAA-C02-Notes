CloudFront
    1.  CloudFront is AWS's Content Delivery Network (CDN) product.
    2.  CloudFront is a global object cache (CDN)
    3.  Content is cached in locations close to customer
    4.  Lower latency and higher throughput
    5.  It can handle static and dynamic content
    6.  CloudFront Terms
        1.  Origin - The source location of your content
        2.  Distribution - the 'Configuration' unit of CloudFront
        3.  Edge Location -  Local infrastructure which hosts a cache of your data
        4.  Regional Edge Cache - Larger version of an edge location.  Provides another layer of caching.
    7.  No write caching.  Any uploads go directly to origin, CDN for download only.
    8.  Caching Optimisation
        1.  Query String Parameters (everything has to match) could lead to removing the benefits of CDN
        2.  Getting the most out of cloudfront requires you to correctly configure what is used for caching and what is not, and what is forwarded to the origin and what isn't.
    9.  Used for Globally static and or dynamic HTTP/S content
    10.  OIA and Bucket policies are used to ensure only CloudFront can access S3 Buckets.
AWS Certificate Manager (ACM)
    1.  The AWS certificate Manage is a service which allows the creation, management and renewal of certificates. It allows deployment of certificates onto supported AWS services such as CloudFront and ALB.
    2.  Create, renew and deploy certificates with ACM
    3.  Supported AWS services ONLY (e.g. CloudFront and ALBs..NOT EC2)
    4.  Certificate created for DNS name with the ACM and deployed onto supported services.  The edge locations also get the certificate.
    5.  Edge locations are accessed using HTTPS and DNS name on the certificate
    6.  Edge locations communicate with the Origin using same protocol.  If HTTPs is used the Origin must have valid trusted certificates.  (no self signed certs)
    7.  ACM must be created in the region in which the service that uses it.
    8.  When using CloudFront ACM must be created in us-east-1 region. 
Securing CF and S3 using OAI
    1.  Origin Access Identities are a feature where virtual identities can be created, associated with a CloudFront Distribution and deployed to edge locations.
    2.  Access to an s3 bucket can be controlled by using these OAI's - allowing access from an OAI, and using an implicit DENY for everything else.
    3.  They are generally used to ensure no direct access to S3 objects is allowed when using private CF Distributions.
    4.  Best pratice is to use one OAI per distribution
  Global Accelerator
    1.  Anycast IP's allow a single IP to be in multiple locations.  Routing moves traffic to closest location
    2.  Traffic initially uses the public internet and enters a Global Accelerator edge location
    3.  From the edge, data transits globally across the AWS global backbone network.  Less hops, directly under AWS control, significantly better performance.
    4.  AWS Global Accelerator is designed to improve global network performance by offering entry point onto the global AWS transit network as close to customers as possible using ANycast IP addresses
    5.  Moves the AWS network closer to customers
    6.  Connections enter at edge ... using anycasts IPs
    7.  Transit over AWS backbone to 1+ locations
    8.  Can be used for NON HTTP/S----used for TCP/UDP*** Difference from CloudFront***


