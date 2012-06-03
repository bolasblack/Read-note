# 版本控制之道——使用 Git
## 第三章 创建第一个项目
### 3.5 处理发布
#### 给指定分支添加 tag

``` bash
$ git tag tagName branchName
```

这样会在分支 branchName 的末梢打上 tagName 的 tag

#### 变基命令

```bash
$ git rebase branchName
```

变基的意思是“改变分支的基底”，把一条分支上的修改在另一条分支的末梢重现。与此同时，rebase 还有重写历史的用途。

#### 给 tag 打补丁

```bash
$ git branch branchName tagName
```

#### 为代码发布创建归档文件

```bash
$ git archive --format tar \
              --prefix=mysite-1.0/ 1.0 \
              | gzip > mysite-10.tar.gz
```

`--format` 指明要产生 tar 格式的输出；`--prefix` 指明包中所有东西都放到 mysite-1.0/ 目录下；`1.0` 指明需要归档的标签名称。

也可以用

```bash
$ git archive --format zip \
              --prefix=mysite-1.0/ 1.0 \
              > mysite-10.zip
```

来产生一个 zip 文件。

## 第四章 添加与提交：Git 基础
### 4.1 添加文件到暂存区
命令 `git add -p` 能够在不进入交互模式的情况下直接进入补丁模式。

补丁模式是 Git 中非常有用的模式，在补丁模式中 Git 会显示这些文件的当前内容与版本库的差异，然后可以据此决定是否添加这些修改到暂存区。

### 4.2 提交修改
命令 `git commit -a` 会把已经纳入 Git 版本控制的文件提交，而不会添加尚未被跟踪的文件。

### 4.3 查看修改内容
`git diff` 默认会比较工作目录树和暂存区的区别，因此如果已经把文件加入了暂存区，那么 `git diff` 命令是无法看到任何 diff 信息的。这时可以使用 `git diff --cached` 命令来比较暂存区和版本库之间的区别。

### 4.4 管理文件
#### 文件的重命名与移动
命令 `git mv <old name> <new name>` 打包了 `mv <old name> <new name>` 、 `git add <new name>` 和 `git rm <old name>`  三个操作。

## 第五章 理解和使用分支
### 5.3 合并分支间的修改
#### 直接合并 (straight merge)

```bash
$ git merge <branchName>
```

#### 压合合并 (squashed commits)
压合指的是 Git 将一条分支上的所有历史提交压合成一个提交，提交到另一条分支上。_要小心使用压合提交_，因为大多数情况下，每个提交都应该作为一个单独的条目存在于历史记录中。

如果某条分支上的所有提交都密切相关，应随后作为一个正题记录（在父分支上）时，适合作压合提交。

```bash
$ git merge --squash <branchName>
```

这条命令会将 branchName 分支上的全部提交压合成当前分支上的一个提交，但这个时候，这些改变还没有提交，需要再次输入 `git commit` 命令进行提交。

#### 拣选合并 (cherry-picking)

```bash
$ git cherry-pick <commit SHA-1>
```

中间跳过了几个 commit ，可能会要求处理冲突。可以带上 -n 参数，用于拣选多个提交，进行合并操作，等全部拣选完毕后输入 `git commit` 命令提交。

### 5.6 分支重命名
命令 `git branch -m <old name> <new name>` 可以用来重命名分支。

## 第六章 查询 Git 历史记录
### 6.2 指定查找范围
在 `git log` 后面可以跟上以下参数用于指定查找范围：

* `--since` 最近的时间内的提交
* `--before` 多少时间前的最后一个提交
* `<commit1 SHA-1>...<commit2 SHA-1>` 检索从 commit1 到 commit2 之间的提交（不包括 commit1）

`-since` 和 `--before` 参数接受大多数英文格式的日期。Git 工具本身能够识别诸如 "24 hours"、"1 minute"、"2008-10.01" 这样格式的日期，哪怕日期中间既有连字符又有句点。

第三种检索方式可以省略 `<commit2 SHA-1>` ，这时 commit2 就会被 HEAD 替代，commit SHA-1 也可以用 tag 名替代。

另一种指定版本的常见方法是指出它与另一版本的关系，此时有两种操作符可以使用：

* ^ 相当于回溯一个版本。`2222222^` 相当于 2222222 的前一个版本； `2222222^^` 代表 2222222 的前第二个版本（也可以写作 `2222222^2`，`^` 其实是 `^1` 的别名）
* ~N 回溯 N 个版本。 `2222222^` 相当于 `222222~1`

可以混合使用两种操作符，

