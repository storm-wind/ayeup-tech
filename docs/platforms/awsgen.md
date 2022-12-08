# AWS General

---

### AWS Technical Essentials

AWS Technical Essentials
- S3 umlimited number of objects
- no bucket size limit
- HTTP/S enpoint to store and retrieve data
- Optional server side encryption
- Access logs for auditing
- Stores data as objects within buckets
- Flat hierarchy structure
	- eg https://doc.s3.amazonaws.com/2006-03-01/AmazonS3.html
- Can upload and download via SSL
- Bucket name must be globally unique
- Objects are denied by default with the permissions
<blank>

##### Access permissions on S3
- Define policy statements (there are examples in the policy generator)


##### Amazon EBS Storage
- Same availability zone as EC2 instance When creating EBS volume
- Use cases
	- OS - Use for boot/root vol, seocndary volumes
	- Databases
	- Enterprise apps
##### Shared Responsibility - AWS (Security)
- Need to look into policy scripting for AWS, similiar for user access and general policy access
- IAM has no associated credentials
- IAM can be dynamically assigned
- Can use application authentication to be trusted in AWS environment to access particular resources?
- 
##### Database Services
- Push button deployment (RDS)
- EBS storage used
- Can take snapshots to different AWS regions
- Provides backups for DR
##### Amazon DynamoDB
- No limit on data
- Quick performance
- provision and change the request capacity for each table
- Fully managed, NOSQL database service


---



### Over-ride VPC DNS

DNS for EC2's in that VPC are determined by DHCP Sets, whcih define the default DNS any EC2's will use.

On an ad-hoc basis with a few EC2's or when using IPSEC tunnel and want to use internal network DNS I can do the following on an EC2 to force it to use local DNS from internal LAN or any other DNS server I specify.

Create or modifiy this file, `/etc/dhcp/dhclient.conf`

Add the following so it looks like this, as an example.

```
timeout 300;
supersede domain-name-servers 10.130.22.21, 10.130.22.22;
```

Modify `/etc/sysconfig/network-scripts/ifcfg-eth0`

Make sure the **PEERDNS=yes** is set.

Reboot and good to go.

More info on DNS for EC2 here, https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html


---



