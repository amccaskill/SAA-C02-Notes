## Security Depolyment and Operations

•	AWS Secrets Manager
            i.	AWS Secrets manager is a product which can manage secrets within AWS. There is some overlap between it and the SSM Parameter Store - but Secrets manager is specialized for secrets.
            ii.	Designed for secrets (..passwords, API KEYS..) *
            iii.	Usable via Console, CLI, API or SDK’s (integration) *
            iv.	Supports automatic rotation … This uses lambda *
            v.	Directly integrates with some AWS products

•	AWS WAF and Shield
         a.	AWS Shield and Web Application Firewall (WAF) are both products which provide perimeter defense for AWS networks
        b.	Shield provides DDOS protection and WAF is a Layer 7 Application Firewall.
        c.	Shield
            i.	Provides AWS resources with DDoS protection *
            ii.	Shield Standard -  free with Route53 and CloudFront *
            iii.	Protection against Layer 3 and Layer 4 DDos Attacks
            iv.	Shield Advanced - $3000 p/m
            v.	EC2, ELB, CloudFront, Global Accelerator and R53 *
            vi.	DDos Response Team and Financial Insurance *
         d.	WAF
            •	Layer 7 (HTTP/s) Firewall
            •	Protects against complex layer 7 attacks/exploits
            •	SQL injections, Cross-Site Scripting, Geo Blocks,   Rate Awareness
             •	Web Access Control (WEBACL) integrated with ALB, API Gateway and CloudFront
            •	Rules are added to a WEBACL and evaluated when traffic arrives

CloudHSM
            an AWS provided Hardware Security Module products.
            True "Single Tenant" Hardware Security Module  *

            CloudHSM is required to achieve compliance with certain security standards such as FIPS 140-2 Level 3 (KMS is L2 overall, some L3) *

            With KMS .. AWS Manage .. Shared but separated
            AWS provisioned  ... fully customer managed

            Industry standard APIs - PKCS#11, Java Cryptography Extensions (JCE), Microsoft CryptoNG (CNG) libraries *

            KMS can use CloudHSM as custom key store, CloudHSM integration with KMS

            Use Cases:
                            No Native AWS integration ... e.g. no S3 SSE
                            Offload the SSL/TLS Processing for Web Servers
                            Enable Transparent Data Encryption (TDE) for Oracle Databases
                            Protect the Private Keys for an Issuing Certificate Authority (CA)

AWS Config
            Record configuration changes over time on resource
            Auditing of changes, compliance with standards
            Does not prevent changes happening ... no protection
            Regional Service supports cross-region and account aggregation
            Changes can generate SNS notification and near-realtime events via EventBridge and Lambda

            EventBridge can invoke lambda functions based on AWS conf events for automatic resource remediation

            Everytime a change occurs a (CI) configuration item is create for that resource.  (point in time and relationship)
            CI form a configuration history            

Amazon Macie
            Amazon Macie is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover and protect your sensitive data in AWS.

            Discover, Monitor, and Protect Data ... stored in S3 Buckets
            Automated discovery of data i.e PII, PHI, Finance
            Managed Data Identifieers - Built-in - ML/Patterns
            Custom Data Identifiers - Proprietary - Regex Based
            Integrates - With Security Hub and "finding events" to EventBridge
            Centrally manage .. either via AWS or one Macie Account Inviting

            Policy Finding or Sensitive findings





