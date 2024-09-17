---
title : "Khởi tạo Vault"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1. </b> "
---
Giờ đây chúng ta đã có thể truy cập vào dịch vụ Vault thông qua UI hoặc Vault Cli, tuy nhiên ta chưa thể sử dụng được các chức năng của Vault, do hiện tại Vault chưa được khởi tạo. Do đó bước đầu tiên chúng ta cần khởi tạo Vault.
#### Khởi tạo Vault
{{% notice note %}}
Để khởi tạo Vault chúng ta có thể thao tác thông qua UI hoặc Vault Cli, tuy nhiên bài lab này sẽ hướng dẫn các bạn khởi tạo Vault sử dụng Vault Cli.
{{% /notice %}}
- Tải Vault Cli rồi kiểm tra version
  ```
  vault version
  ```
- Thêm các biến môi trường cần thiết
  ```
  export VAULT_ADDR="LabALB-137099917.us-east-1.elb.amazonaws.com"
  ```
- Kiểm tra lại trạng thái của Vault
  ```
  vault status
  ```
- Thực hiện khởi tạo Vault
  ```
  vault operator init
  ```
- Bạn cần lưu trữ và bảo quản cẩn thận thông tin 5 keys này và thông tin root token. 
  - Có thể sử dụng dịch vụ AWS KMS để lưu trữ thông tin.
  - **Chú ý** root token không bị revoke nên rất nguy hiểm khi để lộ vào tay người khác.

Lúc này ta đã khởi tạo xong, tuy nhiên ta vẫn chưa thể sử dụng Vault để quản lý, truy cập các secrets do trạng thái lúc này của Vault đang là "seal", tại trạng thái này toàn bộ chức năng của Vault đã bị niêm phong --> Do đó ta cần thực hiện thêm một thao tác là "unseal"

#### Vault Unseal
- Trên terminal, ta nhập lệnh 
  ```
  vault unseal 
  ```
- Sau đó lấy thông tin của 3 key bất kì trong 5 keys để tiến hành unseal, ta nhập liên tiếp cho đến khi Vault được unseal, kết quả như bên dưới
- Kiểm tra lại trạng thái của Vault
  ```
  vault status
  ```
Lúc này Vault đã được unseal, ta đã có thể sử dụng các chức năng của Vault.
  
#### Vault Auto Unseal
Để không mất thời gian nhập key và lưu trữ key an toàn hơn, ta có thể cấu hình Vault để thực hiện auto-unseal kết hợp sử dụng dịch vụ AWS KMS.