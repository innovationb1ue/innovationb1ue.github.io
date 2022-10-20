---
title: wanpei
catalog: true
date: 2022-09-17 23:01:03
subtitle:
header-img: image-20220917205825912.png
tags:
- Golang

categories:
- category1
---

### 玩配 - 20220917 登陆前后端联调 + 匹配入口 + 用户添加游戏tags



#### 前端

主要前后端接口对齐，实现了登陆跳转逻辑与用户注册

![image-20220917193357352](image-20220917193357352.png)

![image-20220917193414689](image-20220917193414689.png)



###  鉴权

完善一下登陆跳转功能。使用SWR获取目前的登陆用户。

https://swr.vercel.app/zh-CN/docs/getting-started

每一个组件都可以使用 useCurrentUser 来获取当前用户信息

>
>
>```javascript
>const {res, isLoading, isError} = useCurrentUser()
>const router = useRouter()
>if (!isLoading && !isError){
>const user = res.data as API.CurrentUser
>console.log("user id = ", user["Gorm.Model"].ID)
>if (!user["Gorm.Model"].ID && user["Gorm.Model"].ID < 0){
>   alert("请先登陆")
>   router.push("/user/login")
>}
>}
>```

暂时实现了匹配页面的检测，后面考虑抽象为一个中间件或者全局状态



## 问题：

如果有小伙伴有好的登陆鉴权例子请一定分享给我，next-auth看了一下不是太懂....暂时老土的用上面的办法实现了。肯定还有很多BUG T_T



### 用户选择游戏界面设计

页面操作左侧选择游戏，右侧进行匹配设置。

目前用FlexBox 和 MUI 的container box 搭了个模型。

左边应该后端还要设置一个游戏列表接口，游戏列表从后端数据库里获取。

![image-20220917205825912](image-20220917205825912.png)
