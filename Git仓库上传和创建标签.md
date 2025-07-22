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
## git 创建标签
在 Git 中，标签（tag）通常用于标记项目中的重要里程碑，比如发布版本。Git 支持两种类型的标签：轻量标签（lightweight）和附注标签（annotated）。附注标签包含了更多的信息，如标签名、打标签的人、日期等，并且可以添加一个标签信息。以下是创建这两种标签的方法：

### 创建标签

#### 1. 创建附注标签
这是最常用的标签类型，它会存储更多的信息。

```bash
git tag -a v1.0 -m "my version 1.0"
```
- `v1.0` 是标签的名字。
- `-m` 参数后面的字符串是标签的信息，类似于提交信息。

#### 2. 创建轻量标签
轻量标签实际上就是一个指向特定提交的引用，不包含额外的信息。

```bash
git tag v1.0-lw
```
- 这里我们创建了一个名为 `v1.0-lw` 的轻量标签，不需要使用 `-a` 和 `-m` 参数。

### 查看标签

查看所有标签：
```bash
git tag
```

查看某个具体标签的信息（仅适用于附注标签）：
```bash
git show v1.0
```

### 推送标签到远程仓库

默认情况下，`git push` 不会推送标签到远程仓库。你需要显式地推送标签：

推送单个标签：
```bash
git push origin v1.0
```

推送所有标签：
```bash
git push origin --tags
```

