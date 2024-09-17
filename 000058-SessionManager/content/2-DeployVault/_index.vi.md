---
title : "Triển khai Vault trên AWS"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

Ta sẽ triển khai Vault trên AWS dạng đơn giản nhất mà vẫn bảo đảm kết và an toàn bảo mật. Ta sẽ cần các thành phần sau:
- 1 Private Subnet, 1 Public Subnet
- 1 NAT Gateway, 1 Internet Gateway.
- 1 Bastion intance ở Public subnet
- 1 Vault instance ở Private subnet.
- 1 ALB để có thể truy cập Vault từ ngoài Internet. 


Tổng quan kiến trúc sau khi hoàn tất các bước này sẽ là:
![Vault architecture](/images/2.prerequisite/vault-architecture.png)

### Nội dung
  - [Chuẩn bị VPC và EC2 Instance](2.1-createec2/)
  - [Tạo IAM Role](2.2-createiamrole/)

  
