# Git 规范

## 功能驱动开发
["功能驱动式开发"](https://en.wikipedia.org/wiki/Feature-driven_development)（Feature-driven development，简称FDD）。
它指的是，需求是开发的起点，先有需求再有功能分支（feature branch）或者补丁分支（hotfix branch）。完成开发后，该分支就合并到主分支，然后被删除。

## [git flow](https://gitversion.readthedocs.io/en/latest/git-branching-strategies/gitflow/)

![5b2e150ba88ac](https://i.loli.net/2018/06/23/5b2e150ba88ac.png)

*主要分支（长期分支）*

```html
* master
* develop
```

短分支

```html
* feature branch 功能分支
* hotfix branch  补丁分支
* release branch 预发（发布）分支
```

优点：清晰可控、工具（Sourcetree）支持

缺点：复杂、分支切换频繁、持续性发布（或者说迭代频繁的）项目结合差

## git commit messsage 规范

每次提交，Commit message 都包括三个部分：Header，Body 和 Footer

```html
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

其中，Header 是必需的，Body 和 Footer 可以省略

### Header
Header 部分只有一行，包括三个字段：type(必需)、scope(可选)和 subject(必需)

**type**  用于说明 commit 的类别，只允许使用下面7个标识

- feat 新功能（feature）
- fix 修补bug
- docs 文档（documentation）
- style 格式（不影响代码运行的变动）
- refactor 重构（即不是新增功能，也不是修改bug的代码变动）
- test 增加测试
- chore 构建过程、辅助工具的变动
- perf 提高性能

如果 **type** 为 `feat` 和 `fix`，则该 commit 将肯定出现在 Change log 之中。
还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以revert:开头，后面跟着被撤销 Commit 的 Header, Body部分的格式是固定的，必须写成`This reverts commit <hash>`.，其中的hash是被撤销 commit 的 SHA 标识符

```
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02
```

**scope** 用于说明 commit 影响的范围，比如数据层、控制层、视图层或子库改动等等，视项目不同而不同。

**subject** 是 commit 目的的简短描述，不超过50个字符
- 以动词开头，使用第一人称现在时，比如 change，而不是 changed 或 changes
- 第一个字母大写
- 结尾不加句号

### Body
Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例

```
More detailed explanatory text, if necessary.  Wrap it to
about 72 characters or so.

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```

应该说明代码变动的动机，以及与以前行为的对比，参考前文提到的 注释要回答如下信息
### Footer 
footer 部分只用于不兼容变动和关闭 Issue

```
Closes #123, #245, #992
```

```
BREAKING CHANGE: isolate scope bindings definition has changed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
    }

    After:

    scope: {
      myAttr: '@',
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

## 注释要回答如下信息

### 为什么这次修改是必要的？

要告诉 Reviewers，你的提交包含什么改变。让他们更容易审核代码和忽略无关的改变。

### 如何解决的问题？

这可不是说技术细节。看下面的两个例子：

```
Introduce a red/black tree to increase search speed

```

```
Remove <troublesome gem X>, which was causing <specific description of issue introduced by gem>

```
如果你的修改特别明显，就可以忽略这个。

### 这些变化可能影响哪些地方？

这是你最需要回答的问题。因为它会帮你发现在某个 branch 或 commit 中的做了过多的改动。一个提交尽量只做1，2个变化。

你的团队应该有一个自己的行为规则，规定每个 commit 和 branch 最多能含有多少个功能修改。

## 小提示

使用 fix, add, change 而不是 fixed, added, changed
永远别忘了第2行是空行
用 Line break 来分割提交信息，让它在某些软件里面更容易读
请将每次提交限定于完成一次逻辑功能。并且可能的话，适当地分解为多次小更新，以便每次小型提交都更易于理解