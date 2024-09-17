---
title : "Tạo Vault Instance"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 2.7 </b> "
---

1. Truy cập [giao diện quản trị dịch vụ EC2](https://console.aws.amazon.com/ec2/v2/home)
  + Click **Instances**.
  + Click **Launch instances**.
  
  
2. Tại trang **Launch instances**
  + Tại mục **Name** ta điền tên instance: "Lab Bastion"
  + Tại mục **Application and OS Image**, chọn AMI **Ubuntu**
  
![](/images/2.prerequisite/vault-01.png)

  + Tại mục **Instance type**, chọn **t2.micro**
  + Tại mục **Key pair**, chọn 1 key pair có sẵn hoặc tạo mới một key pair
  
![](/images/2.prerequisite/vault-02.png)

  + Tại mục **Network setting**,
    + Chọn **VPC** là **LabVPC**
    + Chọn **Subnet** là **Lab Private subnet**
    + Chọn **Auto-assign public IP** là Disable
    + Chọn **Securitu group** là **Lab SG Vault**

![](/images/2.prerequisite/vault-03.png)

    + Mục **Config storage** để mặc định size root volume là **20Gib gp3**
    + Tại mục **User data**. Ta thêm nội dung file script sau để cấu hình, cài đặt Vault:
  ```
  #!/bin/bash
  # Cập nhật gói và nâng cấp hệ thống
  sudo apt-get update -y
  sudo apt-get upgrade -y

  # Cài đặt các gói phụ thuộc
  sudo apt-get install -y unzip curl

  # Tải và cài đặt Vault từ HashiCorp
  VAULT_VERSION="1.14.0"

  curl -O https://releases.hashicorp.com/vault/${VAULT_VERSION}/vault_${VAULT_VERSION}_linux_amd64.zip

  # Giải nén Vault
  unzip vault_${VAULT_VERSION}_linux_amd64.zip

  # Di chuyển Vault đến thư mục hệ thống
  sudo mv vault /usr/local/bin/

  # Cấp quyền thực thi
  sudo chmod +x /usr/local/bin/vault

  # Tạo thư mục cấu hình cho Vault
  sudo mkdir -p /etc/vault.d

  # Tạo file cấu hình cơ bản cho Vault
  cat <<EOF | sudo tee /etc/vault.d/vault.hcl
  storage "file" {
    path = "/opt/vault/data"
  }

  listener "tcp" {
    address     = "0.0.0.0:8200"
    tls_disable = 1
  }

  ui = true
  EOF

  # Tạo thư mục để lưu dữ liệu cho Vault
  sudo mkdir -p /opt/vault/data

  # Tạo service systemd cho Vault
  cat <<EOF | sudo tee /etc/systemd/system/vault.service
  [Unit]
  Description="HashiCorp Vault - A tool for managing secrets"
  Documentation=https://www.vaultproject.io/
  After=network-online.target
  Wants=network-online.target

  [Service]
  ExecStart=/usr/local/bin/vault server -config=/etc/vault.d/vault.hcl
  ExecReload=/bin/kill --signal HUP $MAINPID
  Restart=on-failure
  LimitNOFILE=65536
  StandardOutput=syslog
  StandardError=syslog
  SyslogIdentifier=vault

  [Install]
  WantedBy=multi-user.target
  EOF

  # Reload và khởi động Vault
  sudo systemctl daemon-reload
  sudo systemctl start vault
  sudo systemctl enable vault

  # Mở cổng tường lửa (nếu đang sử dụng UFW)
  sudo ufw allow 8200
  ```
  + Kéo xuống dưới và chọn **Launch Instance**

![](/images/2.prerequisite/vault-04.png)

Như vậy chúng ta đã thực hiện xong việc tạo EC2 Vault trên private subnet.