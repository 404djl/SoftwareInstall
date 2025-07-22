要将本地Git仓库上传到GitHub，需按以下步骤操作：

### 前提条件
1. 已安装Git并配置好用户名和邮箱
2. 已在GitHub上创建空仓库（不要初始化README、LICENSE等文件）

### 操作步骤

#### 1. 初始化本地仓库（若尚未初始化）
在项目根目录下执行：
```bash
git init
```

#### 2. 添加文件并提交到本地仓库
```bash
git add .           # 添加所有文件到暂存区
git commit -m "初始提交"  # 提交到本地仓库
```

#### 3. 关联GitHub远程仓库
```bash
git remote add origin git@github.com:你的用户名/仓库名.git
# 例如：git remote add origin git@github.com:doubao/example-repo.git
```

#### 4. 推送本地分支到GitHub
```bash
git push -u origin main
# 若使用master分支：git push -u origin master
```

### 注意事项
- 确保使用SSH密钥连接GitHub（推荐），否则需使用HTTPS并输入账号密码
- 若遇到权限问题，需在GitHub账户中添加SSH公钥
- 若远程仓库非空，需先拉取合并：`git pull origin main --allow-unrelated-histories`

完成以上步骤后，本地仓库内容将同步到GitHub。后续提交只需执行：
```bash
git add .
git commit -m "更新说明"
git push
```
