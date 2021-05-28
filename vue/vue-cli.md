# 模式和环境变量(使用 Vue CLI 创建的项目)

## 模式

模式是 Vue CLI 项目中一个重要的概念。默认情况下，一个 Vue CLI 项目有三个模式：

development 模式用于 `vue-cli-service serve`

test 模式用于 `vue-cli-service test:unit`

production 模式用于 `vue-cli-service build` 和 `vue-cli-service test:e2e`

你可以通过传递 --mode 选项参数为命令行覆写默认的模式。例如，如果你想要在构建命令中使用开发环境变量：

`vue-cli-service build --mode development`

当运行 `vue-cli-service` 命令时，所有的环境变量都从对应的环境文件中载入。如果文件内部不包含 `NODE_ENV` 变量，它的值将取决于模式，例如，在 production 模式下被设置为 "production"，在 test 模式下被设置为 "test"，默认则是 "development"。

## 环境变量

```bash
.env                # 在所有的环境中被载入
.env.local          # 在所有的环境中被载入，但会被 git 忽略
.env.[mode]         # 只在指定的模式中被载入
.env.[mode].local   # 只在指定的模式中被载入，但会被 git 忽略
```
**不要在应用程序中存储任何机密信息（例如私有 API 密钥）！**

**环境变量会随着构建打包嵌入到输出代码，意味着任何人都有机会能够看到它。**

请注意，只有 `NODE_ENV`，`BASE_URL` 和以 `VUE_APP_` 开头的变量将通过 webpack.DefinePlugin 静态地嵌入到客户端侧的代码中

被载入的变量将会对 vue-cli-service 的所有命令、插件和依赖可用。

## 在客户端侧代码中使用环境变量

```javascript
console.log(process.env.VUE_APP_SECRET)
```

除了 VUE_APP_* 变量之外，在你的应用代码中始终可用的还有两个特殊的变量：

`NODE_ENV` - 会是 "development"、"production" 或 "test" 中的一个。具体的值取决于应用运行的模式。

`BASE_URL` - 会和 vue.config.js 中的 publicPath 选项相符，即你的应用会部署到的基础路径。