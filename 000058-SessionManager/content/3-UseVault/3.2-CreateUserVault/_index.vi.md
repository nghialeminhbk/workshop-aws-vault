---
title : "Tạo người dùng trong Vault"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 3.2. </b> "
---
Giờ ta đã có thể sử dụng các chức năng của Vault, tuy nhiên hiện tại ta chỉ có **root token** cho tài khoản root. Tài khoản này có đầy đủ tất cả các quyền nên hạn chế việc sử dụng tài khoản này khi làm việc với Vault --> Do đó việc đầu tiên cần làm là **tạo người dùng** với các chính sách hạn chế.

#### Tạo người dùng trong Vault
Dưới đây là các bước để tạo người dùng trong Vault với phương pháp xác thực **User/Pass**
- Đăng nhập vào Vault sử dụng **root token** (là token được khởi tạo ở bước init)
  ```
  vault login
  ```
- Bật phương pháp xác thực Userpass
  ```
  vault auth enable userpass
  ```
- Thêm người dùng với Userpass
  ```
  vault write auth/userpass/users/<username> password=<password>

  ```
- Tạo policy cho người dùng để cấp quyền truy cập và tạo **aws scretes**
  - Tạo file policy (aws-policy.hcl)
   ```
   path "aws/*" {
      capabilities = ["read", "list", "update"]
   }
   ```
  - Nạp policy vào Vault
   ```
   vault policy write aws-policy aws-policy.hcl
   ```
- Đăng nhập với người dùng vừa tạo bằng Vault Cli
  ```
  vault login -method=userpass username=<username> password=<password>
  ```
- Đăng nhập với người dùng vừa tạo bằng UI
  - Ta chọn phương thức xác thực là User/Pass
  - Điền thông tin user, pass ta vừa tạo.
  
