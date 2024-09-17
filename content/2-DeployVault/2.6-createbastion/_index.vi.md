---
title : "Tạo Bastion Instance"
date :  "`r Sys.Date()`" 
weight : 6
chapter : false
pre : " <b> 2.6 </b> "
---

1. Truy cập [giao diện quản trị dịch vụ EC2](https://console.aws.amazon.com/ec2/v2/home)
  + Click **Instances**.
  + Click **Launch instances**.
  
2. Tại trang **Launch instances**
  + Tại mục **Name** ta điền tên instance: "Lab Bastion"
  + Tại mục **Application and OS Image**, chọn AMI **Ubuntu**
  + Tại mục **Instance type**, chọn **t2.micro**
  + Tại mục **Key pair**, chọn 1 key pair có sẵn hoặc tạo mới một key pair
  + Tại mục **Network setting**,
    + Chọn **VPC** là **LabVPC**
    + Chọn **Subnet** là **Lab Public subnet**
    + Chọn **Auto-assign public IP** là Enable
    + Chọn **Securitu group** là **Lab SG Bastion**
    + Mục **Config storage** để mặc định size root volume là **8Gib gp3**
    + Kéo xuống dưới và chọn **Launch Instance**

Tiếp theo chúng ta sẽ thực hiện tương tự để tạo 1 EC2 Vault chạy trong Private subnet.