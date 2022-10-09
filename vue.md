# 创建第一个工程化 Vue 项目

> 该项目主要从重写 MyTestTools 工具开始

这里通过 Vue CLI 建一个新的 Vue 项目，之后使用 Element Plus 框架来完成几个简单的页面

## 要求

需要安装 Node 环境，因为需要使用 `@vue/cli`,这里的 Node 版本推荐 v10 以上



## 开始

先查看 Node 是否安装好, 可以使用 `node -v` 来查看具体的安装版本，如果没有则需要安装一下

```sh
# node -v
v14.4.0
```

安装 `@vue/cli`

```
npm install -g @vue/cli
```

安装完成后可以用 `vue -V` 查看版本

```
# vue -V
@vue/cli 4.5.15
```

## 创建项目

在自己喜欢的位置执行 `vue create my_test_tool_vue` 

```
Vue CLI v4.5.15
? Please pick a preset:
  Default ([Vue 2] babel, eslint)
❯ Default (Vue 3) ([Vue 3] babel, eslint)
  Manually select features
```

选择 Vue 3 那一项

如果一切顺利，则会有类似下面的提示：

```
🎉  Successfully created project my_test_tool_vue.
👉  Get started with the following commands:
```

> 如果因为网络原因导致运行期间出现错误的提示，比如 Error 之类的提示，可以先删除  my_test_tool_vue ，然后再次执行。

