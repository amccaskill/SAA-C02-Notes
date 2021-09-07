
EC2 Placement Groups

1. Cluster Placement Groups (PERFORMANCE)
    1. Instance should  be same type (Not manditory)  and launched at the same time.
    2. Same Rack Sometimes Same EC2 instance Host
    3. Direct bandwidth to same instance in same cluster group single stream transfer rate 10 Gbps
    4. Lowest latency and max PPS possible in AWS
    5. Can’t span Availability Zone
    6. Can span VPC peers -  but impacts performance
    7. Requires a supported instance type
2. Spread Placement Groups (Resilience)
    1. Design to ensure the maximum amount of availability and resilience for an application
    2. Can span multiple AZ 
    3. Isolated distinct racks, power and network in each AZ
    4. Limit 7 instance per AZ
    5. Provides the highest level of resilience
3. Partition Placement Groups (Topology Awareness)
    1. Similar to spread placement groups
    2. When you have more than 7 instances per AZ, But still require the ability to separate those instances into separate fault domains
    3. Partitions group across multiple AZ
        1.  maximum of 7 partitions per AZ
        2. Each partition has its own racks and no sharing between
    4. You have a element of control of which partition that your instance goes into
    5. Not supported on dedicated host

EC2 Dedicated Hosts

1. Host dedicated to you
2. Specific family e.g. a1, c5, m5
3. No instance charges you only pay for the host
4. On-Demand and reserved options available
5. Host hardware has physical sockets and cores
6. AMI Limits no support for RHEL, SUSE Linus, and windows AMI
7. Amazon RDS instances are not supported
8. Placement groups are not supported
9. Host can be shared with other ORG account...RAM
10. Generally used for applications with software licensing. Not usually used.

Enhanced Networking and EBS Optimized
    1. Enhanced Networking
        1. Uses SR-IOV-NIC is virtualization aware
        2. NIC is virtualization aware
        3. Higher I/O and Lower Host CPU
        4. More Bandwidth
        5. Higher packet per second PPS
        6. Consistent lower latency
        7. No charge available on most EC2 Types
    2. EBS Optimized
        1. EBS = Block storage over the network
        2. Historically network was shared.. data and EBS
        3. EBS optimized means dedicated capacity for EBS
            1. Most instances support and have enabled by default
            2. Some support, but enabling costs extra