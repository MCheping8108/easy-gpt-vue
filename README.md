# 🌟easy-gpt-vue
利用vite+vue写出来简易的GPT网页，你可以自己DIY这个GPT网页，还能把DIV的网页打包成软件

# 🚀快速开始
> ⚠友情提醒⚠: 请把`.env.template`重命名为`.env`并配置需要的环境变量
## 🏠本地部署
下载代码
```bash
git clone git@github.com:MCheping8108/easy-gpt-vue.git
```

安装依赖
```bash
npm install
```

在本地开发(仅适用于开发)
```bash
npm run dev
```

编译
```bash
npm run build
```

开启服务器
```bash
npm run preview
```

打包软件
```bash
npm run app:build
```

# 📷截图
![Alt text](./docs/images/image3.png)

# ☕未来计划
- [x] 正式支持`.env`环境变量
- [x] 支持markdown
- [x] 支持输入文字回答
- [x] 支持切换模型
- [x] 支持密码输入
- [x] 将ChatGPT回答保存至本地
- [x] 适配手机端
- [x] 支持移动端应用(不能自动签名)
- [ ] 接入kimiAI(傻逼CORS策略把我搞到土坑活埋了)

# Android/IOS

本地开发
```bash
npm run tauri android/ios dev
```

打包
```bash
npm run tauri android/ios build
```