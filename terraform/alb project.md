# 🚀 AWS ALB + ASG Setup with Home and Cloth Applications

This Terraform project provisions an **Application Load Balancer (ALB)** with two Auto Scaling Groups (ASGs), serving:
- **Home App** at `/`
- **Cloth App** at `/cloth`

---

## 📂 Project Structure

```text
mini project

├── main.tf                   # Core infrastructure resources
├── variables.tf              # Variables for easy customization
├── output.tf                 # Outputs ALB DNS name
├── terraform.tfstate         # Terraform state (auto-generated)
├── README.md                  # Project documentation


## ✅ Features
- Secure Security Group (HTTP, SSH access)
- Home and Cloth applications served by separate ASGs
- ALB with listener and routing rules
- Health checks for Target Groups
- CloudWatch alarms + Scaling Policies
- Outputs ALB DNS name for access

---

## ⚡ Prerequisites

- Terraform installed ([Install Terraform](https://developer.hashicorp.com/terraform/downloads))
- AWS CLI configured with credentials
- Existing SSH keypair in AWS (`key_name` in variables)

---

## ⚙️ Variables

```hcl
variable "ami" {
  default = "ami-0360c520857e3138f"
  type    = string
}

variable "instance_type" {
  default = "t2.micro"
  type    = string
}

variable "availability_zones" {
  default = ["us-east-1a", "us-east-1b", "us-east-1c"]
}

variable "key_name" {
  default = "saurav"
}
````

---

## ✅ Full Terraform Code

### Provider and Data Sources

```hcl
provider "aws" {
  region = "us-east-1"
}

data "aws_vpc" "default" {
  default = true
}

data "aws_subnets" "default" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.default.id]
  }
}
```

### Security Group

```hcl
resource "aws_security_group" "my_sg" {
  name        = "my-sg"
  description = "Allow HTTP(80) and SSH(22) from anywhere; allow all outbound"
  vpc_id      = data.aws_vpc.default.id

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
    description = "HTTP"
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
    description = "SSH"
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    description = "All outbound"
  }

  tags = {
    Name = "my-sg"
  }
}
```

### Launch Templates

```hcl
resource "aws_launch_template" "launch_templet_home" {
  name          = "aws_launch_template_home"
  instance_type = var.instance_type
  vpc_security_group_ids = [aws_security_group.my_sg.id]
  key_name      = var.key_name
  image_id      = var.ami

  user_data = base64encode(<<-EOF
#!/bin/bash
apt update -y
apt install -y nginx
echo "<h1>welcome home</h1>" > /var/www/html/index.html
systemctl enable nginx
systemctl restart nginx
EOF
  )

  tag_specifications {
    resource_type = "instance"
    tags = {
      Name = "home-instance"
    }
  }

  tags = {
    Name = "templet-home"
  }
}

resource "aws_launch_template" "launch_templet_cloth" {
  name          = "aws_launch_template_cloth"
  instance_type = var.instance_type
  vpc_security_group_ids = [aws_security_group.my_sg.id]
  key_name      = var.key_name
  image_id      = var.ami

  user_data = base64encode(<<-EOF
#!/bin/bash
apt update -y
apt install -y nginx
mkdir -p /var/www/html/cloth
echo "<h1>cloth</h1>" > /var/www/html/cloth/index.html
systemctl enable nginx
systemctl restart nginx
EOF
  )

  tag_specifications {
    resource_type = "instance"
    tags = {
      Name = "cloth-instance"
    }
  }

  tags = {
    Name = "templet-cloth"
  }
}
```

### Application Load Balancer

```hcl
resource "aws_lb" "alb" {
  name               = "alb-main"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.my_sg.id]
  subnets            = data.aws_subnets.default.ids

  enable_deletion_protection = false

  tags = {
    Name = "alb-main"
  }
}
```

### Target Groups

```hcl
resource "aws_lb_target_group" "tg_home" {
  name        = "tg-home"
  port        = 80
  protocol    = "HTTP"
  vpc_id      = data.aws_vpc.default.id
  target_type = "instance"

  health_check {
    path     = "/index.html"
    port     = "80"
    protocol = "HTTP"
  }
}

resource "aws_lb_target_group" "tg_cloth" {
  name        = "tg-cloth"
  port        = 80
  protocol    = "HTTP"
  vpc_id      = data.aws_vpc.default.id
  target_type = "instance"

  health_check {
    path     = "/cloth/index.html"
    port     = "80"
    protocol = "HTTP"
  }
}
```

### Listener and Listener Rule

```hcl
resource "aws_lb_listener" "http_listener" {
  load_balancer_arn = aws_lb.alb.arn
  port              = 80
  protocol          = "HTTP"

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.tg_home.arn
  }
}

resource "aws_lb_listener_rule" "cloth_rule" {
  listener_arn = aws_lb_listener.http_listener.arn
  priority     = 200

  action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.tg_cloth.arn
  }

  condition {
    path_pattern {
      values = ["/cloth*", "/cloth/*"]
    }
  }
}
```

### Auto Scaling Groups

```hcl
resource "aws_autoscaling_group" "asg_home" {
  name                    = "asg_home"
  min_size                = 2
  max_size                = 3
  desired_capacity        = 2
  vpc_zone_identifier     = data.aws_subnets.default.ids
  health_check_type       = "ELB"
  health_check_grace_period = 120

  launch_template {
    id      = aws_launch_template.launch_templet_home.id
    version = "$Latest"
  }

  target_group_arns = [aws_lb_target_group.tg_home.arn]

  tag {
    key                 = "Name"
    value               = "asg-home-instance"
    propagate_at_launch = true
  }
}

resource "aws_autoscaling_group" "asg_cloth" {
  name                    = "asg_cloth"
  min_size                = 2
  max_size                = 3
  desired_capacity        = 2
  vpc_zone_identifier     = data.aws_subnets.default.ids
  health_check_type       = "ELB"
  health_check_grace_period = 120

  launch_template {
    id      = aws_launch_template.launch_templet_cloth.id
    version = "$Latest"
  }

  target_group_arns = [aws_lb_target_group.tg_cloth.arn]

  tag {
    key                 = "Name"
    value               = "asg-cloth-instance"
    propagate_at_launch = true
  }
}
```

### Scaling Policies and CloudWatch Alarms

```hcl
# Home ASG Policies
resource "aws_autoscaling_policy" "policy_home_up" {
  name                   = "policy_home_up"
  autoscaling_group_name = aws_autoscaling_group.asg_home.name
  adjustment_type        = "ChangeInCapacity"
  scaling_adjustment     = 1
  cooldown               = 120
}

resource "aws_autoscaling_policy" "policy_home_down" {
  name                   = "policy_home_down"
  autoscaling_group_name = aws_autoscaling_group.asg_home.name
  adjustment_type        = "ChangeInCapacity"
  scaling_adjustment     = -1
  cooldown               = 120
}

resource "aws_cloudwatch_metric_alarm" "cw_alarm_home_up" {
  alarm_description   = "Scale up home ASG when CPU >= 70%"
  alarm_actions       = [aws_autoscaling_policy.policy_home_up.arn]
  alarm_name          = "home_scale_up"
  comparison_operator = "GreaterThanOrEqualToThreshold"
  namespace           = "AWS/EC2"
  metric_name         = "CPUUtilization"
  threshold           = 70
  evaluation_periods  = 2
  period              = 60
  statistic           = "Average"

  dimensions = {
    AutoScalingGroupName = aws_autoscaling_group.asg_home.name
  }
}

resource "aws_cloudwatch_metric_alarm" "cw_alarm_home_down" {
  alarm_description   = "Scale down home ASG when CPU <= 15%"
  alarm_actions       = [aws_autoscaling_policy.policy_home_down.arn]
  alarm_name          = "home_scale_down"
  comparison_operator = "LessThanOrEqualToThreshold"
  namespace           = "AWS/EC2"
  metric_name         = "CPUUtilization"
  threshold           = 15
  evaluation_periods  = 5
  period              = 60
  statistic           = "Average"

  dimensions = {
    AutoScalingGroupName = aws_autoscaling_group.asg_home.name
  }
}

# Cloth ASG Policies
resource "aws_autoscaling_policy" "policy_cloth_up" {
  name                   = "policy_cloth_up"
  autoscaling_group_name = aws_autoscaling_group.asg_cloth.name
  adjustment_type        = "ChangeInCapacity"
  scaling_adjustment     = 1
  cooldown               = 120
}

resource "aws_autoscaling_policy" "policy_cloth_down" {
  name                   = "policy_cloth_down"
  autoscaling_group_name = aws_autoscaling_group.asg_cloth.name
  adjustment_type        = "ChangeInCapacity"
  scaling_adjustment     = -1
  cooldown               = 120
}

resource "aws_cloudwatch_metric_alarm" "cw_alarm_cloth_up" {
  alarm_description   = "Scale up cloth ASG when CPU >= 70%"
  alarm_actions       = [aws_autoscaling_policy.policy_cloth_up.arn]
  alarm_name          = "cloth_scale_up"
  comparison_operator = "GreaterThanOrEqualToThreshold"
  namespace           = "AWS/EC2"
  metric_name         = "CPUUtilization"
  threshold           = 70
  evaluation_periods  = 2
  period              = 60
  statistic           = "Average"

  dimensions = {
    AutoScalingGroupName = aws_autoscaling_group.asg_cloth.name
  }
}

resource "aws_cloudwatch_metric_alarm" "cw_alarm_cloth_down" {
  alarm_description   = "Scale down cloth ASG when CPU <= 15%"
  alarm_actions       = [aws_autoscaling_policy.policy_cloth_down.arn]
  alarm_name          = "cloth_scale_down"
  comparison_operator = "LessThanOrEqualToThreshold"
  namespace           = "AWS/EC2"
  metric_name         = "CPUUtilization"
  threshold           = 15
  evaluation_periods  = 5
  period              = 60
  statistic           = "Average"

  dimensions = {
    AutoScalingGroupName = aws_autoscaling_group.asg_cloth.name
  }
}
```

### Output

```hcl
output "alb_dns_name" {
  description = "ALB DNS name"
  value       = aws_lb.alb.dns_name
}
```

---

## 🏃 Commands

### 1️⃣ Initialize Terraform

```bash
terraform init
```

### 2️⃣ Plan Infrastructure

```bash
terraform plan
```

### 3️⃣ Apply Configuration

```bash
terraform apply
```

Example output:

```text
alb_dns_name = "alb-main-xxxxxxxxxx.us-east-1.elb.amazonaws.com"
```

### 4️⃣ Access Applications

* Home App: `http://<alb_dns_name>/`
* Cloth App: `http://<alb_dns_name>/cloth/`

---

## 🧹 Cleanup

```bash
terraform destroy
```

---
