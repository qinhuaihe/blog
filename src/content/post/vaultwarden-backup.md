---
title: "Vaultwarden Backup"
publishDate: "2 February 2024"
description: "This post is talk about how to automatically backup Vaultwarden."
tags: ["Bitwarden", "Vaultwarden", "backup", "rclone", "Docker"]
---

# Reference
[ttionya/vaultwarden-backup](https://github.com/ttionya/vaultwarden-backup/)

# 配置rclone
# 此命令会创建一个名为`vaultwarden-rclone-data`的[数据卷](https://yeasy.gitbook.io/docker_practice/data_management/volume)

```
sudo docker run --rm -it \
  --mount type=volume,source=vaultwarden-rclone-data,target=/config/ \
  ttionya/vaultwarden-backup:latest \
  rclone config
```

# 查看rclone配置
```
sudo docker run --rm -it \
  --mount type=volume,source=vaultwarden-rclone-data,target=/config/ \
  ttionya/vaultwarden-backup:latest \
  rclone config show
```
  
# 运行vaultwarden_backup
```
sudo docker run -d \
  --restart=always \
  --name vaultwarden_backup \
  --volumes-from=vaultwarden \
  --mount type=volume,source=vaultwarden-rclone-data,target=/config/ \
  -e DATA_DIR="/data" \
  ttionya/vaultwarden-backup:latest
```
