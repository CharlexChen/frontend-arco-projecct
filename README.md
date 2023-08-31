# 用户身份认证系统

## 本地开发

```shell
# 若没安装pnpm命令，则先执行
npm install -g pnpm

# 安装依赖
pnpm install

# 启动服务
pnpm dev

```

## 项目备注

### 登陆页面

访问`http://localhost:5173/login`时，如下：

`index.html` -> `src/main.ts` -> `src/App.vue` -> `src/views/login/index.vue`

### 登陆表单组件

路径：`src/views/login/components/login-form.vue`

登陆方法：`handleSubmit`

### 前端服务请求代理配置

当请求打到前端项目服务的端口时，匹配到前缀是`/api`则会将请求转发到`http://localhost:3000`

```typescript
{
    // ...
    server: {
      // ...
      proxy: {
        '/api': {
          target: 'http://localhost:3000',
          changeOrigin: true,
          rewrite: (path) => path.replace(/^\/api/, ''),
        },
      },
    },
    // ...
}
```
