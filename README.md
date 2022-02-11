# Migrate-to-Amazon-RDS.docx
 
1.aws ec2 describe-vpcs --vpc-ids vpc-08bbcf5d2d01233d7 \
--filters "Name=tag:Name,Values= Cafe VPC" \
--query "Vpcs[*].CidrBlock"

2.Cafe VPC IPv4 CIDR block: 10.200.0.0/20

3.aws ec2 describe-subnets \
--filters "Name=vpc-id,Values=vpc-08bbcf5d2d01233d7" \
--query "Subnets[*].[SubnetId,CidrBlock]"

4. aws ec2 describe-availability-zones \
--filters "Name=region-name,Values=us-west-2" \
--query "AvailabilityZones[*].ZoneName"

5. http://ec2-54-188-143-105.us-west-2.compute.amazonaws.com/cafe

6.Number of orders: 69.50

7. aws ec2 create-security-group \
--group-name CafeDatabaseSG \
--description "Security group for Cafe database" \
--vpc-id vpc-08bbcf5d2d01233d7

8. aws ec2 authorize-security-group-ingress \
--group-id sg-03eb6a0143fe1d34e \
--protocol tcp --port 3306 \
--source-group sg-06ada420b78735450

9. aws ec2 describe-security-groups \
--query "SecurityGroups[*].[GroupName,GroupId,IpPermissions]" \
--filters "Name=group-name,Values='CafeDatabaseSG'"

10.aws ec2 create-subnet \
--vpc-id vpc-08bbcf5d2d01233d7 \    10.200.2.0/23
--cidr-block 10.200.10.0/23 \
--availability-zone us-west-2b


11.aws ec2 create-subnet \
--vpc-id vpc-08bbcf5d2d01233d7 \    10.200.10.0/23
--cidr-block 10.200.2.0/23 \
--availability-zone us-west-2a


12.aws rds create-db-subnet-group \
--db-subnet-group-name "CafeDB Subnet Group" \
--db-subnet-group-description "DB subnet group for Cafe" \
--subnet-ids subnet-04cb3f8b0646e435a subnet-00f36209897299bf4 \
--tags "Key=Name,Value= CafeDatabaseSubnetGroup"

13.  aws rds create-db-instance \
--db-instance-identifier CafeDBInstance \
--engine mariadb \
--engine-version 10.5.13 \
--db-instance-class db.t3.micro \
--allocated-storage 20 \
--availability-zone us-west-2a \
--db-subnet-group-name "CafeDB Subnet Group" \
--vpc-security-group-ids sg-03eb6a0143fe1d34e \
--no-publicly-accessible \
--master-username root --master-user-password 'Re:Start!9'

14. mysqldump --user=root --password='Re:Start!9' \
--databases cafe_db --add-drop-database > cafedb-backup.sql

15. less cafedb-backup.sql--- q to go back

16. mysql --user=root --password='Re:Start!9' \
--host=cafedbinstance.cepdvniinorb.us-west-2.rds.amazonaws.com \
< cafedb-backup.sql

17.mysql --user=root --password='Re:Start!9' \
--host=cafedbinstance.cepdvniinorb.us-west-2.rds.amazonaws.com \
cafe_db

18.select * from product;

19. http://ec2-54-188-143-105.us-west-2.compute.amazonaws.com/cafe

20. mysql --user=root --password='Re:Start!9' \
--host=ec2-54-188-143-105.us-west-2.compute.amazonaws.com \
cafe_db

21.select * from product;

22.exit


