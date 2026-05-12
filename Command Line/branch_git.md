<div align="center" style="margin-bottom: 15px;">
  <img src="../assets/tcq-protocol(4).png" width="400" height="auto">
</div>

## Basic Command Line (Làm việc với Local)

### 1. Liệt kê tất cả các branch hiện có ở local
```bash
git branch
```

### 2. Tạo một branch mới nhưng không tự động chuyển sang branch đó
```bash
git branch <branch_name>
```

### 3. Chuyển sang làm việc trên branch khác
```bash
git checkout <branch_name>
```

### 4. Vừa tạo new_branch vừa chuyển sang branch đó
```bash
git checkout -b <branch_name>
```

### 5. Đổi tên branch hiện tại đang đứng
```bash
git branch -m <new_name>
```
```bash
# Đổi tên branch từ old_name sang new_name
git branch -m <old_name> <new_name> 
```

### 6. Xóa branch local (chỉ xóa được khi đã hợp nhất vào branch chính)
```bash
git cherry-pick <commit-code>
```
* Dùng `-D` nếu muốn xóa luôn branch đó dù chưa merge.

### 7. Lấy duy nhất một commit cụ thể từ Branch khác về branch hiện tại
```bash
git branch -d <branch_name>
```

## Basic Command Line (Làm việc với Remote)

### 1. Đẩy branch từ Local lên Remote
```bash
git push origin <branch_name>
```

### 2. Xóa một branch đang tồn tại trên Remote
```bash
git push origin --delete <branch_name>
```

### 3. Cập nhật danh sách các new_branch từ Remote về Local (nhưng chưa gộp vào src)
```bash
git fetch
```

### 4. Kéo code từ branch trên Remote và gộp luôn vào branch hiện tại ở local
```bash
git pull origin <branch _name>
```

### 5. Dọn dẹp! Xóa các bản ghi tracking của các branch đã bị xóa trên Remote nhưng vẫn còn hiện diện trong danh sách Local
```bash
git remote prune origin
```

## Hợp nhất và Tái cấu trúc (Merge & Rebase)
### 1. Gộp src từ branch khác vào branch hiện tại
```bash
git merge <branch_name>
```

### 2. Gộp src từ branch khác vào branch hiện tại
```bash
git merge <branch_name>
```