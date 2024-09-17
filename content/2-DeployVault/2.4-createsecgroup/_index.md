---
title : "Tạo các security group"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4. </b> "
---

#### Tạo các security group

Trong bước này chúng ta sẽ tiến hành tạo các security group được sử dụng cho các instance của chúng ta. Sẽ có 3 đối tượng cần tạo security group trong bài lab này:
- Application Load Balancer
- Bastion Instance
- Vault Instance

#### Tạo security group cho Bastion instance nằm trong public subnet 

1. Truy cập [giao diện quản trị dịch vụ VPC](https://console.aws.amazon.com/vpc)
  + Click **Security Group**.  
  + Click **Create security group**.

![SG](/images/2.prerequisite/019-createsg.png)

2. Tại mục **Security group name**, điền **Lab SG Bastion**. 
  + Tại mục **Description**, điền **SG Bastion**.
  + Tại mục **VPC**, click dấu **X** để chọn lại **Lab VPC** bạn đã tạo cho bài lab này.

![SG](/images/2.prerequisite/020-createsg.png)

3. Thêm **Inbound rule** để cho phép SSH từ Internet vào Bastion
  

4. Giữ nguyên **Outbound rule**, kéo chuột xuống phía dưới.
  + Click **Create security group**.


#### Tạo security group cho Vault instance nằm trong private subnet 

1. Sau khi tạo thành công security group cho Bastion instance nằm trong public subnet, click vào link Security Groups để quay trở lại danh sách Security groups.

![SG](/images/2.prerequisite/021-createsg.png)

2. Click **Create security group**.

3. Tại mục **Security group name**, điền **Lab SG Vault**. 
  + Tại mục **Description**, điền **SG Vault**.
  + Tại mục **VPC**, click dấu **X** để chọn lại **Lab VPC** bạn đã tạo cho bài lab này.

![SG](/images/2.prerequisite/022-createsg.png)

4. Thêm **Inbound Rule**
  + Cho phép SSH từ **Bastion**
  + Cho phép traffic từ **Application Load Balancer**

5. Kéo chuột xuống phía dưới.
  + Thêm **Outbound rule** cho phép kết nối tới Internet (All traffic | 0.0.0.0/0)
  + Click **Create security group**.

![SG](/images/2.prerequisite/023-createsg.png)


#### Tạo security group cho Application Load Balancer

1. Trong bước này, chúng ta sẽ tạo security group cho Application Load Balancer. Mục đích Load Balancer này sẽ là route traffic từ Internet tới các Vault Instance --> Để cho người dùng có thể sử dụng dịch vụ Vault.
2. Sau khi tạo thành công security group cho Vault instance trong private subnet, click vào link Security Groups để quay trở lại danh sách Security groups.
3. Click **Create security group**.
4.  Tại mục **Security group name**, điền **Lab SG ALB**. 
  + Tại mục **Description**, điền **SG ALB**.
  + Tại mục **VPC**, click dấu **X** để chọn lại **Lab VPC** bạn đã tạo cho bài lab này.

![SG](/images/2.prerequisite/024-createsg.png)

5. Thêm **Inbound rules** cho phép traffic từ Internet. (Allow TCP | 0.0.0.0/0)
   
6. Thêm **Outbound rules** 
  + Xóa **Outbound rule** cho phép TCP 8200 đến Vault Instance.
  + Click **Create security group**.
  
![SG](/images/2.prerequisite/025-createsg.png)

Như vậy chúng ta đã tiến hành xong việc tạo các security group cần thiết cho các Bastion instance và Vault Instance.