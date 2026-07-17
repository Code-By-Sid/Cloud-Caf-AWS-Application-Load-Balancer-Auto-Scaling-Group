# ☕ Cloud Café – High Availability Web Application on AWS

## 📌 Project Overview

Cloud Café is a hands-on AWS project that demonstrates how to build a **highly available**, **fault-tolerant**, and **scalable** web application using **Amazon EC2**, **Application Load Balancer (ALB)**, and **Auto Scaling Group (ASG)**.

The application is hosted on multiple EC2 instances running **Nginx**, with traffic distributed through an Application Load Balancer. Auto Scaling ensures the desired number of instances are always available and automatically scales based on CPU utilization.

---

# 🏗️ Architecture

```text
                    Internet Users
                           │
                           ▼
              Application Load Balancer
                           │
                    Target Group (HTTP)
                           │
          ┌────────────────┴────────────────┐
          │                                 │
      EC2 Instance 1                   EC2 Instance 2
      Amazon Linux                     Amazon Linux
         Nginx                            Nginx
          │                                 │
          └──────── Auto Scaling Group ─────┘
                 Min: 2 | Desired: 2 | Max: 4
```

---

# 🚀 AWS Services Used

* Amazon EC2
* Amazon Linux 2023
* Application Load Balancer (ALB)
* Target Group
* Auto Scaling Group (ASG)
* Launch Template
* Security Groups
* VPC
* CloudWatch (Scaling Metrics)

---

# 🎯 Features

* Deploys a web application on multiple EC2 instances.
* Distributes incoming traffic using an Application Load Balancer.
* Performs health checks to route traffic only to healthy instances.
* Automatically launches new instances during high CPU usage.
* Automatically terminates extra instances when demand decreases.
* Replaces unhealthy or terminated instances automatically (Self-Healing).
* Displays the EC2 Instance ID on the web page to verify load balancing.

---

# 📋 Prerequisites

* AWS Account
* IAM User with EC2 permissions
* Existing EC2 Key Pair
* Basic knowledge of:

  * EC2
  * VPC
  * Security Groups
  * SSH
  * Linux Commands

---

# 📁 Project Components

## 1. EC2 Instances

* Amazon Linux 2023
* t2.micro
* Nginx Web Server
* HTTP (80)
* SSH (22)

---

## 2. Launch Template

Contains:

* Amazon Linux AMI
* Instance Type
* Key Pair
* Security Group
* User Data Script

The User Data script:

* Updates the server
* Installs Nginx
* Starts and enables the Nginx service
* Retrieves the EC2 Instance ID using IMDSv2
* Creates the application homepage

---

## 3. Application Load Balancer

* Internet Facing
* HTTP Listener (Port 80)
* Routes traffic to the Target Group
* Supports multiple Availability Zones

---

## 4. Target Group

Protocol:

```
HTTP
```

Port:

```
80
```

Health Check Path:

```
/
```

Automatically checks whether EC2 instances are healthy before forwarding traffic.

---

## 5. Auto Scaling Group

Configuration:

| Property         | Value |
| ---------------- | ----- |
| Minimum Capacity | 2     |
| Desired Capacity | 2     |
| Maximum Capacity | 4     |

Scaling Policy:

* Target Tracking
* Average CPU Utilization
* Target Value: 50%

---

# ⚙️ Deployment Steps

1. Launch two EC2 instances.
2. Install and configure Nginx.
3. Create a Target Group.
4. Register both EC2 instances.
5. Create an Application Load Balancer.
6. Attach the Target Group.
7. Verify health checks.
8. Create a Launch Template.
9. Add the User Data script.
10. Create an Auto Scaling Group.
11. Attach the Auto Scaling Group to the Target Group.
12. Test load balancing.
13. Simulate instance failure.
14. Verify automatic recovery.
15. Generate CPU load to test automatic scaling.

---

# 🧪 Testing

## Load Balancing

Refresh the ALB DNS URL multiple times.

Expected Result:

* Requests are served by different EC2 instances.
* Instance IDs change as traffic is distributed.

---

## Health Check

Stop Nginx on one instance:

```bash
sudo systemctl stop nginx
```

Expected:

* Target becomes **Unhealthy**.
* ALB stops sending traffic to that instance.

---

## Auto Scaling

Generate CPU load:

```bash
sudo dnf install stress -y
stress --cpu 2 --timeout 300
```

Expected:

* CPU exceeds 50%.
* Auto Scaling launches a new EC2 instance.
* The new instance is automatically registered with the Target Group.

---

## Self-Healing

Terminate one EC2 instance.

Expected:

* Auto Scaling detects the missing instance.
* A replacement instance is launched automatically.
* The new instance passes health checks.
* ALB begins routing traffic to it.

---

# 📂 Project Structure

```text
CloudCafe/
│
├── README.md
├── launch-template-user-data.sh
├── screenshots/
│   ├── ec2-running.png
│   ├── alb.png
│   ├── target-group.png
│   ├── autoscaling-group.png
│   ├── health-check.png
│   └── load-balancing.png
└── architecture.png
```

---

# 📸 Screenshots
# Server 1:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/61aeaa94-b547-4b6b-8afe-824ef9dabb52" />

# Server 2:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f0c8eb62-69be-4ec0-b3a9-a90702f515b8" />

# Web Page:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8f9dcdbb-dc6a-4b38-8f0b-1a5b16ee5c28" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e266d444-37e0-432a-b7dc-6f64dfc5c7fa" />


---

# 📚 Learning Outcomes

After completing this project, you will understand:

* Application Load Balancer (ALB)
* Target Groups
* Health Checks
* Launch Templates
* Auto Scaling Groups
* High Availability
* Fault Tolerance
* Elastic Scaling
* EC2 User Data
* IMDSv2 Metadata Service

---

# 🛠️ Future Enhancements

* Add HTTPS using AWS Certificate Manager (ACM)
* Configure Route 53 with a custom domain
* Deploy a Flask or Node.js application
* Add Amazon RDS for persistent storage
* Store static assets in Amazon S3
* Enable CloudWatch dashboards and alarms
* Integrate AWS CodeDeploy and CodePipeline for CI/CD

---

# 👨‍💻 Author

**Siddhesh More**

AWS | Python | Generative AI | Cloud Engineer

---

# 📄 License

This project is intended for educational and learning purposes.
