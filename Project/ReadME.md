This project involved deploying two EC2 instances behind an Application Load Balancer to serve a web page with high availability and secure traffic routing. Health checks, security groups, and user data scripting were configured to ensure automation, scalability, and controlled access.

## EC2 Instances
I launched two EC2 instances (Amazon Linux 2) in the same VPC, but in two different Availability Zones for high availability and redundancy.
During launch, I used a User Data script to install Apache (httpd), start the service, and create a simple HTML web page that displayed a welcome message and confirmed the instance was running.

## Application Load Balancer (ALB)
I created an Application Load Balancer (ALB) to distribute incoming traffic between the two EC2 instances.

During ALB creation:

- I placed the ALB in two public subnets across different Availability Zones for fault tolerance.
- I selected HTTP on port 80 as the listener.
- I then created a Target Group, set the target type to "instance", and registered both EC2 instances with it.
- I also configured health checks using the / path (which corresponds to the root of the Apache-hosted web page). This allowed the ALB to monitor instance health and only forward traffic to healthy ones.

## Security Groups

ALB Security Group:
- Created a Security Group for the Application Load Balancer (ALB) that allows inbound HTTP (port 80) and HTTPS (port 443) traffic from anywhere (0.0.0.0/0), ensuring the ALB is reachable over both protocols. Attached an HTTPS listener to the ALB to securely handle incoming encrypted traffic.

EC2 Security Group:
- Created a Security Group for the EC2 instances that only allows inbound HTTP (port 80) from the ALB's security group (using the ALB SG ID as the source). This ensures the EC2 instances are not directly accessible from the internet, and only traffic routed through the ALB can reach them.

## End Result 
After setting everything up:
- I accessed the ALB DNS name in a browser (e.g., http://alb-name-123456.eu-west-2.elb.amazonaws.com) and saw the simple Apache web page served from one of the EC2 instances.
- Traffic alternates between the two instances (load balancing).
- Health checks ensured that only healthy EC2 instances receive traffic.
- The architecture is redundant, secure, and scalable.


<img width="600" height="500" alt="image" src="https://github.com/user-attachments/assets/53fbbf0d-f9b5-403e-8087-5c4465ee7b54" />

<img width="959" height="406" alt="image" src="https://github.com/user-attachments/assets/02035e7f-a505-45f5-8a79-edd3359c7f79" />

