# **POC for AMI Creation**

| **Author** | **Created on** | **Version** | **Last updated by**|**Last Edited On**|**Level** |**Reviewer** |
|------------|----------------|-------------|----------------|-----------------|-------------|-------------|
| Sharvari Khamkar|   04-03-2025  | v1          | Sharvari Khamkar   | 04-03-2025  |  Internal Reviewer | Komal Jaiswal | 

---

## **Table of Contents**
- [Introduction](#introduction)
- [Pre-requisites](#pre-requisites)
- [AMI Creation Steps](#ami-creation-steps)
- [Companies Using Packer](#companies-using-packer)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [References](#references)

---

## **Introduction**

![image](https://github.com/user-attachments/assets/af92b37d-1e49-4e59-bfa0-52c96aba4bff)

Packer, developed by HashiCorp, is a popular open-source tool for creating Amazon Machine Images (AMIs). It enables automation, multi-platform support, and infrastructure-as-code principles, simplifying the AMI creation process.

---

## **Pre-requisites**

| **Tool** | **Version** |
|---------|-------------|
| **Packer** | v1.12.0 |

---

## **AMI Creation Steps**

### **1. Create an EC2 Instance and AMI**
- Launch an EC2 instance (`New_Instance`).
- Customize it as needed.
- Create an AMI (`New_Image`).

### **2. Install Packer**
- Download and install Packer from the official website.
  ![1](https://github.com/user-attachments/assets/ead96ed5-446a-46d3-bd12-62bc82ff7b8c)
  ![2](https://github.com/user-attachments/assets/927ff736-ab43-403a-bd91-624fc9560d65) <br>
  
- Verify installation using `packer --version`.
   <br>
  ![3](https://github.com/user-attachments/assets/de039089-5093-48a4-ac92-7ed0b7982676)

### **3. Create a Packer Template**
- Define the AMI configuration in a `nano ubuntu-ami.pkr.hcl
` file.

```hcl
packer {
  required_plugins {
    amazon = {
      version = ">= 1.0.0"
      source  = "github.com/hashicorp/amazon"
    }
  }
}

variable "aws_region" {
  default = "us-east-1"
}

variable "ami_name" {
  default = "packer-ubuntu-instance-image"
}

source "amazon-ebs" "ubuntu" {
  source_ami_filter {
    filters = {
      name                = "ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"
      root-device-type    = "ebs"
      virtualization-type = "hvm"
    }
    owners      = ["099720109477"]
    most_recent = true
  }

  instance_type = "t2.micro"
  ssh_username  = "ubuntu"
  region        = var.aws_region

  ami_name        = var.ami_name
  ami_description = "Ubuntu AMI created using Packer inside an EC2 instance"
}

build {
  sources = ["source.amazon-ebs.ubuntu"]

  provisioner "shell" {
    inline = [
      "sudo apt update"
    ]
  }
}

```

### **4. Initialize and Build the AMI**
- Run `packer init ubuntu-ami.pkr.hcl` to initialize. <br>
  ![4](https://github.com/user-attachments/assets/d2993b3c-0c08-4f44-b76b-20b255580b0d)
  
- Validate the Template `packer validate ubuntu-ami.pkr.hcl`.
- Use `packer build ubuntu-ami.pkr.hcl` to start the AMI creation process. <br>
  ![5](https://github.com/user-attachments/assets/394a5204-4021-4d28-b179-07289f4a823d)
  

- Verify the AMI in the AWS Console.
  ![6](https://github.com/user-attachments/assets/03cbe7cf-431f-4370-91a3-e37e72e22d90)

---

## **Companies Using Packer**

| **Company** | **Usage** |
|------------|----------|
| **HashiCorp** | Uses Packer for DevOps workflows. |
| **Netflix** | Uses automation tools for infrastructure. |
| **GitHub** | Incorporates Packer in provisioning processes. |
| **Adobe** | Uses DevOps tools, including Packer. |

---

## **Conclusion**

Packer is a powerful tool for automating AMI creation, ensuring consistency and scalability. Its integration with other HashiCorp tools makes it a valuable asset for cloud infrastructure management.

---

## **Contact Information**  

| **Name** | **Email**  |  
|---------|------------|  
| Sharvari Khamkar | sharvari.khamkar@mygurukulam.co |  

---

## **References**

| **Source** | **Description** |
|----------|--------------|
| [Pluralsight](https://www.pluralsight.com/cloud-guru/labs/aws/using-packer-to-create-an-ami) | Guide on AMI Setup |
| [HashiCorp Docs](https://developer.hashicorp.com/packer/tutorials/aws-get-started/aws-get-started-build-image) | Packer installation and setup guide |

