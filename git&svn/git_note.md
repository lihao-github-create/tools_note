# git常用操作和问题

## 基本概念

## 常用操作

### 创建一个本地repository

有两种常用的方式，一种是在本地创建并推到远程：

```bash
echo "# Base" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:lihao-github-create/Base.git
git push -u origin main
```

另一个从远程仓库clone到本地：

```bash
git remote add origin git@github.com:lihao-github-create/Base.git
git branch -M main
git push -u origin main
```

## 问题和解决方案
