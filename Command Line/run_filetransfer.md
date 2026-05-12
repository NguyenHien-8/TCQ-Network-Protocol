<div align="center" style="margin-bottom: 15px;">
  <img src="../assets/tcq-protocol(4).png" width="400" height="auto">
</div>

# Command Line Run Filetransfer

## 1. Trường Hợp: Lệnh chạy folder filetransfer

### Lệnh Compile folder filetransfer trên Server
```bash
go build -o tcq-file-server ./example/filetransfer/server
```

### Lệnh Compile folder filetransfer trên Client
```bash
go build -o tcq-file-client.exe .\example\filetransfer\client
```

### Lệnh lắng nghe port :4242 trên Server
```bash
./tcq-file-server -listen 0.0.0.0:4242 -root ./tcq-storage -token "change-me"
```

### Lệnh upload file và folder
```bash
# Upload File:
.\tcq-file-client.exe -server 100.91.211.76:4242 -token "change-me" upload E:\React_Native_CLI\Khao_Sat_Chart.rar
# Upload Folder:
.\tcq-file-client.exe -server 100.91.211.76:4242 -token "change-me" upload E:\React_Native_CLI\Khao_Sat_Chart
```

### Lệnh download file và folder
```bash
# Download File:
.\tcq-file-client.exe -server 100.91.211.76:4242 -token "change-me" download Khao_Sat_Chart.rar E:\Archiver Storage\Storage
# Download Folder:
.\tcq-file-client.exe -server 100.91.211.76:4242 -token "change-me" download Khao_Sat_Chart E:\Archiver Storage\Storage\Khao_Sat_Chart
```

