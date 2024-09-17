---
title : "Tạo Application Load Balancer"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

{{% notice info %}}
ALB trong Lab này ngoài tác dụng cân bằng tải, ta sẽ dùng nó như một forward proxy để người dùng có thể truy cập dịch vụ Vault từ bên ngoài Internet
{{% /notice %}}

#### Tạo Target Groups
1. Truy cập dịch vụ **EC2** và chọn **Target group** trên thanh menu.
2. Tạo mới **Target Group**
  + Chọn **Create target group**
3. Tại trang **Create target group**
  + Ở bước 1 **Specify group details**:
    + Tại mục **Choose a target type**, ta chọn **Instances**
    + Tại mục **Target group name**, điền tên **LabTGVault**
    + Protocol để **HTTP**, Port là **8200**
    + Chọn VPC là **LabVPC**
    + Tại mục **Health checks**, để **Health check protocol** là **HTTP** và **Health check path** là **/ui/**
  + Ở bước 2 **Register targets**
    + Chọn **Lab Vault** Instance
    + **Ports for selected instances** ta điền 8200
    + Chọn **Include as pending below**
  + Chọn **Create target group**

#### Tạo Application Load Balancer
1. Truy cập dịch vụ **EC2** và chọn **Load Balancers** trên thanh menu.
2. Tạo mới **Load Balancer** 
3. Chọn **Create** Application Load Balancer
4. Tại trang **Create Application Load Balancer** 
  + Tại mục **Load Balancer name**, điền tên của Load Balancer "LabALB"
  + Tại mục **Network mapping**, chọn VPC là **LabVPC**
  + Tại mục **Mapping**, chọn 2 AZ trong đó phải có 1 AZ chứa public subnet, và private subnet có Vault Instance
  + Tại mục **Security Group**, chọn security group "Lab SG ALB" ta vừa tạo ở các bước trước
  + Tại mục **Listeners and routing**, ta thêm Listenr với cấu hình như sau:
    + Protocol **HTTP**, Port **80** và chọn target group ta vừa tạo ở bước trên
  + Kéo xuống dưới và chọn **Create load balancer**.
