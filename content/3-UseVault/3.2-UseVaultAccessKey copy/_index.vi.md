---
title : "Sử dụng Vault quản lý AWS Access Key"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3.3. </b> "
---
Tại bước này ta sẽ ứng dụng Vault để giải quyết bài toán quản lý AWS Access Key và tích hợp với AWS Cli hay Terraform,...
#### Quản lý AWS Access Key bằng Vault
- Bật và cấu hình AWS Secret Engine. Vault có một AWS Secrets Engine giúp ta cấp phát tạm thời AWS Access Key và Secret Key.
  - Bật AWS Secrets Engine
   ```
   vault secrets enable -path=aws aws
   ```
  - Cấu hình thông tin xác thực AWS (tại đây bạn điền đầy đủ thông tin vào)
   ```
   vault write aws/config/root access_key=<AWS_ACCESS_KEY> secret_key=<AWS_SECRET_KEY> region=<AWS_REGION>
   ```
  - Tạo role để cấp phát AccessKey tạm thời
    - role: Tên của vai trò bạn tạo.
    - aws_policy_arns: ARN của các quyền mà bạn muốn gán cho các key được tạo ra (ở đây sử dụng AdministratorAccess).
   ```
   vault write aws/roles/<role> credential_type=iam_user policy_arns=<aws_policy_arns>
   ```
   ví dụ:
   ```
   vault write aws/roles/admin-role credential_type=iam_user policy_arns=arn:aws:iam::aws:policy/AdministratorAccess
   ```
- Cấp phát AWS Access Key tạm thời
  ```
  vault read aws/creds/admin-role
  ```

  kết quả trả về như sau:
  ```bash
   Key                Value
   ---                -----
   lease_id           aws/creds/my-role/xxx
   lease_duration     1h
   lease_renewable    true
   access_key         AKIAIOSFODNN7EXAMPLE
   secret_key         wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   security_token     null
  ```
- Giờ ta có thể dùng thông tin **access_key** và **secret_key** để cấu hình AWS Cli
  ```
  aws configure
  ```
  Nhập thông tin **access_key** và **secret_key** lấy từ Vault.
- Để tự động hoá bước này ta có thể viết đoạn script để tự động hoá lấy Access Key và Secret Key từ Vault
   ```bash
   #!/bin/bash

   # Lấy Access Key và Secret Key từ Vault
   AWS_CREDS=$(vault read aws/creds/my-role -format=json)

   # Lấy các giá trị Access Key và Secret Key
   ACCESS_KEY=$(echo $AWS_CREDS | jq -r .data.access_key)
   SECRET_KEY=$(echo $AWS_CREDS | jq -r .data.secret_key)

   # Cấu hình AWS CLI
   aws configure set aws_access_key_id $ACCESS_KEY
   aws configure set aws_secret_access_key $SECRET_KEY

   echo "AWS CLI đã được cấu hình với Access Key tạm thời từ Vault."
  ```
- Xác nhận AWS Cli đã được kết nối đến AWS
  ```
  aws s3 list
  ```
