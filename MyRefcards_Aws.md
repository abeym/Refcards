# MyRefcards_Aws.md

## Common AWS Commands

### ECS (Elastic Container Service)
- List all ECS clusters:
  ```bash
  aws ecs list-clusters
  ```
- Describe a specific ECS cluster:
  ```bash
  aws ecs describe-cluster --cluster <cluster-name>
  ```
- List all services in a cluster:
  ```bash
  aws ecs list-services --cluster <cluster-name>
  ```
- Update a service in a cluster:
  ```bash
  aws ecs update-service --cluster <cluster-name> --service <service-name> --desired-count <count>
  ```

## Update an Existing ECS Service from the Existing Config

1. **Retrieve the current service configuration:**
   ```bash
   aws ecs describe-services --cluster <cluster-name> --services <service-name> > service-config.json
   ```

2. **Modify the configuration file (`service-config.json`) as needed.**
   - Update fields like `desiredCount`, `taskDefinition`, or `networkConfiguration`.

3. **Update the ECS service with the modified configuration:**
   ```bash
   aws ecs update-service --cluster <cluster-name> --service <service-name> --cli-input-json file://service-config.json
   ```

### ALB (Application Load Balancer)
- List all load balancers:
  ```bash
  aws elbv2 describe-load-balancers
  ```
- Describe a specific load balancer:
  ```bash
  aws elbv2 describe-load-balancers --names <load-balancer-name>
  ```
- List target groups:
  ```bash
  aws elbv2 describe-target-groups
  ```
- Register a target with a target group:
  ```bash
  aws elbv2 register-targets --target-group-arn <target-group-arn> --targets Id=<instance-id>
  ```

### Java SDK Examples
- Initialize AWS SDK for Java:
  ```java
  AmazonS3 s3Client = AmazonS3ClientBuilder.standard()
                              .withRegion(Regions.US_EAST_1)
                              .build();
  ```
- List S3 buckets:
  ```java
  List<Bucket> buckets = s3Client.listBuckets();
  for (Bucket bucket : buckets) {
      System.out.println(bucket.getName());
  }
  ```
- Publish a message to an SNS topic:
  ```java
  AmazonSNS snsClient = AmazonSNSClientBuilder.standard()
                              .withRegion(Regions.US_EAST_1)
                              .build();
  
  String message = "Hello, AWS SNS!";
  String topicArn = "arn:aws:sns:us-east-1:123456789012:MyTopic";
  
  PublishRequest publishRequest = new PublishRequest(topicArn, message);
  snsClient.publish(publishRequest);
  ```

### Trace from IP to ECS Service
1. **Find the ALB associated with the IP:**
   - Use the IP to find the DNS name of the ALB:
     ```bash
     nslookup <ip-address>
     ```
   - The output will show the DNS name of the ALB.

2. **Find the Route53 record pointing to the ALB:**
   - List all Route53 hosted zones:
     ```bash
     aws route53 list-hosted-zones
     ```
   - List all records in a hosted zone:
     ```bash
     aws route53 list-resource-record-sets --hosted-zone-id <hosted-zone-id>
     ```
   - Look for a record that matches the ALB DNS name.

3. **Find the Target Group associated with the ALB:**
   - List all target groups:
     ```bash
     aws elbv2 describe-target-groups
     ```
   - Find the target group associated with the ALB by matching the `LoadBalancerArns` field.

4. **Find the ECS Service associated with the Target Group:**
   - List all ECS services in the cluster:
     ```bash
     aws ecs list-services --cluster <cluster-name>
     ```
   - Describe each service to find the one associated with the target group:
     ```bash
     aws ecs describe-services --cluster <cluster-name> --services <service-name>
     ```
     - Look for the `loadBalancers` field in the output.

### Trace from ECS Service to IP
1. **Find the Target Group associated with the ECS Service:**
   - Describe the ECS service:
     ```bash
     aws ecs describe-services --cluster <cluster-name> --services <service-name>
     ```
   - Note the `targetGroupArn` in the `loadBalancers` field.

2. **Find the ALB associated with the Target Group:**
   - Describe the target group:
     ```bash
     aws elbv2 describe-target-groups --target-group-arns <target-group-arn>
     ```
   - Note the `LoadBalancerArns` field.
   - Describe the ALB:
     ```bash
     aws elbv2 describe-load-balancers --load-balancer-arns <load-balancer-arn>
     ```
     - Note the DNS name of the ALB.

3. **Find the Route53 record pointing to the ALB:**
   - List all Route53 hosted zones:
     ```bash
     aws route53 list-hosted-zones
     ```
   - List all records in a hosted zone:
     ```bash
     aws route53 list-resource-record-sets --hosted-zone-id <hosted-zone-id>
     ```
   - Look for a record that matches the ALB DNS name.

4. **Find the IP associated with the ALB:**
   - Use the DNS name of the ALB to find the IP:
     ```bash
     nslookup <alb-dns-name>
     ```
