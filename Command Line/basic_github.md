<div align="center" style="margin-bottom: 15px;">
  <img src="../assets/tcq-protocol(4).png" width="400" height="auto">
</div>

# Command Line Github

### Reset lại remote origin
```bash
git remote set-url origin <new-remote-url>
```

### Tạo Branch mới và push lên remote
```bash
git checkout -b <branch-name>
git push -u origin <branch-name>
```

### Push branch riêng lên GitHub rồi tạo Pull Request
```bash
git push -u origin <branch-name>
```

### Đổi tên branch local
```bash
git branch -m <new-name>
```

### Đổi tên branch remote
```bash
git push origin -u <new-name>
```

## 2. PowerShell Command Windows

### Xóa thư mục bằng lệnh chuẩn của PowerShell
``` powershell
Remove-Item -Path ".\<folder_name>" -Recurse -Force
```
