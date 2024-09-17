---
title : "Tạo Private subnet"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3. </b> "
---

#### Tạo Private subnet

1. Click **Subnets**.
  + Click **Create subnet**.

![VPC](/images/2.prerequisite/017-createsubnet.png)

2. Tại trang **Create subnet**.
  + Tại mục **VPC ID** click chọn **Lab VPC**.
  + Tại mục **Subnet name** điền **Lab Private Subnet**.
  + Tại mục **Availability Zone** chọn Availability zone đầu tiên.
  + Tại mục **IPv4 CIRD block** điền **10.10.2.0/24**.

![VPC](/images/2.prerequisite/018-createsubnet.png)

3. Kéo xuống cuối trang , click **Create subnet**.

#### Tạo Route table cho Private subnet
1. Tại trang **Create route table**.
  + Tại mục **Name**, điền **Lab Private RTB**.
  + Tại mục **VPC**, chọn **Lab VPC**.
  + Click **Create route table**.

2. Sau khi tạo route table thành công.
  + Click **Edit routes**.
  
![VPC](/images/2.prerequisite/012-creatertb.png)

3. Tại trang **Edit routes**.
  + Click **Add route**.
  + Tại mục **Destination** điền 0.0.0.0/0
  + Tại mục **Target** chọn **NAT Gateway** sau đó chọn **Lab NAT**.
  + Click **Save changes**.

![VPC](/images/2.prerequisite/013-creatertb.png)

4. Click tab **Subnet associations**.
  + Click **Edit subnet associations** để tiến hành associate custom routable chúng ta vừa tạo vào **Lab Private Subnet**.


![VPC](/images/2.prerequisite/014-creatertb.png)

1. Tại trang **Edit subnet associations**. 
  + Click chọn **Lab Public Subnet**.
  + Click **Save associations**.

![VPC](/images/2.prerequisite/015-creatertb.png)
Bước tiếp theo chúng ta sẽ tạo các security group cần thiết cho bài lab.