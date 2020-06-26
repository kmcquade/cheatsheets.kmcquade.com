# Cartography/Neo4j

### Internet accessible ALBs

```text
match (lb:LoadBalancer{scheme:"internet-facing"})-[:MEMBER_OF_EC2_SECURITY_GROUP]->(sb:EC2SecurityGroup)<-[:MEMBER_OF_EC2_SECURITY_GROUP]-(IP:IpPermissionInbound{protocol:"tcp"})<-[:MEMBER_OF_IP_RULE]-(rule:IpRange{range:"0.0.0.0/0"}) with lb, sb,IP,rule
match(aws:AWSAccount)-[:RESOURCE]->(lb) return aws.name,lb.dnsname,collect(IP.fromport)
```

### Public Facing EC2 Instances with Instance Profiles attached

```text
MATCH (n:EC2Instance)
WHERE  (n.iaminstanceprofile) starts with 'arn' and (n.publicdnsname) contains '.'
RETURN n.region, n.instanceid, n.iaminstanceprofile, n.publicdnsname
```

## 

