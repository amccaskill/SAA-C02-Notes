Network Storage           
           
            EFS Architecture
            1.  The Elastic File System (EFS) is an AWS managed implementation of NFS which allows for the creation of shared 'filesystems' which can be mounted within multi EC2 instances.
            2.  Network Based File system which can be mounted within linux EC2 instances and used by multiple instances at once 
            3.  EFS is an implementation of NFSv4
            4. EFS Filesystems can be mounted in Linux
            5. Shared between many EC2 instances
            6.  Typically platforms store images, posts, movies, and audio locally.  If the instance is lost so is the media.
            7.  EFS storage exist separately from EC2. 
            8.  Private service, via mount targets inside a VPC
            9.  Can be accessed from on-premise - VPN or DX
            10.  Linux Only
            11.  General Purpose and Max I/O Performance Modes
            12.  General Purpose is default for 99.9 percent of uses
            13.  Bursting and Provisioned Throughput Modes
            14.  Standard and Infrequent Access (IA) Classes
            15.  Lifecycle policies can be used with classes
