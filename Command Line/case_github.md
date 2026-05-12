<div align="center" style="margin-bottom: 15px;">
  <img src="../assets/tcq-protocol(4).png" width="400" height="auto">
</div>

# Command Line Github

## 1. Trường Hợp: Xóa file cached từ file nào đó

### Cài đặt git filter-repo
```bash
# Nếu dùng MacOS (qua Homebrew):
brew install git-filter-repo
# Nếu dùng Windows/Linux (qua Python/pip):
pip install git-filter-repo
```

### Lệnh xóa file/folder cached khỏi lịch sử Local (Phải tạo bản sao lưu)
``` bash
cp -r <ten-thumuc-du-an> <ten-thumuc-du-an-backup>
```

```bash
# <file-path> là đường dẫn thực tế đến file muốn xóa
git filter-repo --invert-paths --path <file-path>
# <folder-path> là đường dẫn thực tế đến folder muốn xóa
git filter-repo --invert-paths --path <folder-path>/
```

### Lệnh xóa file/folder cached khỏi lịch sử Local (Không cần tạo bản sao)
```bash
# <file-path> là đường dẫn thực tế đến file muốn xóa
git filter-repo --invert-paths --path <file-path> --force
# <folder-path> là đường dẫn thực tế đến folder muốn xóa
git filter-repo --invert-paths --path <folder-path>/ --force
```

### Dọn dẹp bộ nhớ đệm (Reflog Cache) ở Local
```bash
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

### Thêm Remote URL và kết nối lại Remote
```bash
git remote add origin <URL_Repo>
```

### Đẩy lịch sử mới lên Remote
```bash
git push origin --force --all
git push origin --force --tags
```

## 2. Trường Hợp: Thay thế đường dẫn module path Golang

### Thay toàn bộ exact module path các src
```bash
$old = 'github.com/NguyenHien-8/TCQ-Network-Protocol'
$new = 'github.com/NguyenHien-8/tcq-network-protocol'

# Quét tất cả file .go và go.mod trong thư mục hiện tại và các thư mục con
Get-ChildItem -Path . -Recurse -File | Where-Object { $_.Extension -eq '.go' -or $_.Name -eq 'go.mod' } | ForEach-Object {
    $path = $_.FullName
    $text = [System.IO.File]::ReadAllText($path)
    
    # Chỉ tiến hành thay thế và ghi đè nếu file thực sự chứa chuỗi cũ
    if ($text.Contains($old)) {
        $text = $text.Replace($old, $new)
        [System.IO.File]::WriteAllText($path, $text)
        Write-Host "Updated: $($_.Name)" -ForegroundColor Green
    }
}
```

### Thay toàn bộ exact module path các src
```bash
git diff --name-only -- '*.go' | ForEach-Object {
    gofmt -w $_
}
```

### Kiểm Tra module path cũ đã hết
```bash
git grep -n 'module_path'
```

###  Bỏ toàn bộ thay đổi khi chưa commit
```bash
git restore .
```

## 3. Trường Hợp: Xóa toàn bộ branch và cached branch đó

### Xóa branch tại Local
```bash
git switch main
git branch -d <branch_name>
git branch
```

### Xóa branch trên Remote
```bash
git push origin --delete <branch_name>
```

### Xóa cached remote-tracking branch cũ
```bash
git fetch --prune origin
```

### Xóa branch (worktree) tại Local
```bash
git worktree list
git worktree remove <branch_path>
git branch -D release-v0.1
```

## 4. Trường Hợp: Đổi branch main sang master

### Xóa cached remote-tracking branch cũ
```bash
git branch -m main master
git push -u origin master
```

### Trên Github
Repository -> Settings -> Branches -> Default branch -> đổi main thành master

### Xóa branch main trên remote
```bash
git push origin --delete main
# Xóa cached những branch đã không tồn tại trên Remote
git fetch --prune origin
# Xác định origin là branch nào 
git remote set-head origin master
```

## 5. Trường Hợp: Đưa toàn bộ src *_test.go đó sang branch riêng và trên branch đó bỏ tên *_test.go

### Tạo backup branch mới trên Local
```bash
git switch -c <branch_name>
```

### Xóa toàn bộ file .go không phải *_test.go
```bash
git ls-files '*.go' | Where-Object { $_ -notlike '*_test.go' } | ForEach-Object {
    git rm $_
}
```

### Xóa toàn bộ file *_test.go không phải .go
```bash
git switch master
git ls-files '*.go' | Where-Object { $_ -notlike '*_test.go' } | ForEach-Object {
    git rm $_
}
```

### Rename toàn bộ *_test.go thành .go
```bash
git ls-files '*_test.go' | ForEach-Object {
    $newName = $_ -replace '_test\.go$', '.go'
    git mv $_ $newName
}
```

### Commit branch và Push branch riêng lên Remote
```bash
git commit -m "description"
git push -u origin <branch_name>
```

## 6. Trường Hợp: Thêm ghi chú cho tất cả các source code

### Tạo backup branch mới trên Local
```bash
git switch -c <branch_name>
```

### Thêm ghi chú vào toàn bộ source .go
```bash
$header = @'
//  Project: QPACK HTTP3
//  Author: Trần Nguyên Hiền (c)
//  Major: Electronic And Communication Engineering
//  Email: trannguyenhien29085@gmail.com
//  Date: 2/3/2026
//  MIT Licence
//
// ----------------------------------------------------------------

'@

$marker = 'Project: QPACK HTTP3'
$utf8NoBom = New-Object System.Text.UTF8Encoding -ArgumentList $false

# Sử dụng Get-ChildItem để quét toàn bộ file .go trên ổ cứng
Get-ChildItem -Path . -Filter *.go -Recurse -File | ForEach-Object {
    $path = $_.FullName
    $text = [System.IO.File]::ReadAllText($path)

    # Nếu file đã có ghi chú này rồi thì bỏ qua, không thêm trùng lặp
    if ($text.Contains($marker)) {
        return
    }

    [System.IO.File]::WriteAllText($path, $header + $text, $utf8NoBom)
    Write-Host "Add a note: $($_.Name)" -ForegroundColor Green
}
```

### Format lại source
```bash
gofmt -w (git ls-files '*.go')
```

### Kiểm tra thay đổi
```bash
git diff --stat
git diff -- buffer_pool.go
```

### Kiểm tra file nào chưa có header
```bash
git ls-files '*.go' | ForEach-Object {
    $text = [System.IO.File]::ReadAllText($_)
    if (-not $text.Contains('Project: TCQ Network Protocol')) {
        $_
    }
}
```