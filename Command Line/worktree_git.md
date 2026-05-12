<div align="center" style="margin-bottom: 15px;">
  <img src="../assets/tcq-protocol(4).png" width="400" height="auto">
</div>

* Thay vì chỉ làm việc tại 1 branch duy nhất (phải commit rồi mới chuyển sang branch khác). Thì worktree cho phép mở nhiều branch cùng lúc trong các thư mục khác nhau nhưng vẫn chung một bộ máy Git quản lý.

## Basic Command Line

### 1. Tạo một folder mới riêng cho branch
```bash
git worktree add <path> <branch_name>
```

### 2. Tạo branch mới và mở nó trong folder mới luôn
```bash
git worktree add -b <branch_name> <path>
```

### 3. Tạo branch rỗng (Empty/Orphan Branch)
```bash
git worktree add --orphan -b <branch-name> <path>
```

### 4. Liệt kê tất cả các worktree hiện đang hoạt động
```bash
git worktree list
```

### 5. Xóa một worktree (Lưu ý: Chỉ xóa thư mục và liên kết, không xóa nhánh)
```bash
git worktree remove <branch_path>
```

### 6. Di chuyển thư mục làm việc sang một vị trí khác
```bash
git worktree move <old_branch_path> <new_branch_path>
```

### 7. Khóa một worktree lại
* Hửu ích khi worktree đó nằm trên một ổ đĩa mạng hoặc ổ cứng rời, giúp Git không vô tình dọn dẹp `prune` nó khi thiết bị không kết nối.  
```bash
git worktree lock <branch_path>
```

### 8. Mở khóa một worktree
```bash
git worktree unlock <branch_path>
```

### 9. Quét và xóa sạch các cached này trong metadata của Git
* Nếu lỡ tay xóa thư mục worktree bằng lệnh `rm -rf` thay vì dùng lệnh của git, hệ thống sẽ vẫn nghĩ worktree đó còn tồn tại thì dùng `prune`.
```bash
git worktree prune
```

## Advanced Command Line
### 1. Tạo một worktree tại một thời điểm commit cụ thể trong quá khứ để kiểm tra lỗi mà không cần tạo branch mới
```bash
git worktree add --checkout <branch_path> <commit-hash>
```

### 2. Tạo một worktree ở trạng thái "detached HEAD"
* Rất hữu ích khi chỉ muốn chạy thử code hoặc chạy test tự động mà không có ý định commit thêm gì vào branch đó.
```bash
git worktree add --detach <branch_path> <branch_name>
```