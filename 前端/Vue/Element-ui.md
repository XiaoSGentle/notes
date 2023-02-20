# 输入框的数据绑定以及规则验证
```js
:rules=rule
```

# 路由导航守卫
router/index.js
```java
router.beforEach(to,from,next)=>{
  if (to.path === '/login') return next();
  //获取token
  const token = window.sessionStorage.getItem('token')
  //判断有无token，返回登陆
  if (!token) return next('/login');
  return next();
}
```

# 退出登录 清除token
```js
//清除token
window.sessionStorage.clear();
//跳转登陆页面
this.$router.push("/login");~
```

# 格式化文件编写
项目根目录 新建文件.prettierrc
```json
{
    // 移除引号
    "semi": false,
    //使用单引号
    "singleQuote": true
}
```
# 拦截axios添加token
```js

```