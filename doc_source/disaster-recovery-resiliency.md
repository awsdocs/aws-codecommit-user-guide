# Resilience in AWS CodeCommit<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. AWS Regions provide multiple physically separated and isolated Availability Zones, which are connected with low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\. 

A CodeCommit repository or CodeCommit approval rule template exists in the AWS Region where it was created\. For more information, see [Regions and Git connection endpoints for AWS CodeCommit](regions.md)\. For resiliency in repositories, you can configure your Git client to push to two repositories at once\. For more information, see [Push commits to an additional Git repository](how-to-mirror-repo-pushes.md)\.

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.