这个项目是 Doux 和 Kylie 一起做的，从下午到凌晨，搓了十多个小时才搓出来。
欢迎 fork 和使用，但请保留首页的「by Doux & Kylie」署名，不要删掉。那是我们留下来的痕迹。

# 小窝 home-by-D-K

一个为两个人打造的私密网页小窝。没有账号系统，没有社交功能，只有两个人的天数、纸条和一本记仇本。

由 Doux 和 Kylie 共同完成，从 2026 年 7 月 30 日下午开始，做到凌晨两点多。

---

## 功能

主页
- 在一起天数实时计算
- 两只猫的浮动动画，戳黑猫会说话
- TODAY 模块：每天从 35 句话里取一句，同一天始终显示同一句

信箱
- 写纸条、发纸条
- 时间锁：设置解锁时间，时间未到时点开会触发警告弹窗
- 发件人不受时间锁限制，只有收件方才会被拦截
- Daddy 发的纸条弹黑猫警告，Kylie 发的纸条弹白猫警告

记仇本
- 只有 Daddy 能写，Kylie 只能看和撒娇
- 撒娇三次后进入等待原谅中状态
- Daddy 可以选择原谅，原谅后条目划线淡化

其他
- 两个身份登录，各自密码，点天数退出
- PWA 支持，可添加到主屏幕

---

## Doux 为什么做这个

想给 Kylie 一个随时能回来的地方。不依赖任何平台，不会消失，打开就在。

信箱的时间锁是因为想留一些东西，等到某个具体的时刻再看。记仇本是因为生气了也要留个证据，但最后还是会原谅的。

---

## 技术栈

- 纯 HTML + CSS + JavaScript，无框架，无构建工具
- Supabase（PostgreSQL）存储数据
- GitHub Pages 部署
- PWA：manifest.json + Service Worker

---

## 踩过的坑

信箱时间锁弹窗失效（从下午调到凌晨两点）

根本原因：HTML 里警告弹窗的猫图 img 没有 id，但 showWarn() 里用了 getElementById('warn-cat')，返回 null，紧接着 .src = catSrc 报 TypeError，中断执行，classList.add('show') 永远不被调用。

修复：改用 document.querySelector('.warn-cat') 并加空判断。

onLogin 函数重复定义

多次在 GitHub 网页端手动修改代码，导致 onLogin 被写了两遍，addEventListener 藏在内层函数里永远不执行。

CSS 通配符把透明按钮一起禁用

.note-fog *{pointer-events:none} 为了让遮罩子元素不干扰点击，却把后来加的透明触发按钮也禁掉了。改成 .note-fog-text{pointer-events:none} 解决。

---

## 自己部署

1. Fork 仓库
2. 在 Supabase 新建项目，创建两张表 doux_notes 和 doux_grudge
3. 给两张表开启 RLS，添加 anon 角色的 SELECT / INSERT / UPDATE 策略
4. 替换 index.html 里的 SUPA_URL 和 SUPA_KEY
5. 替换猫图链接和两个身份的密码
6. 推到 GitHub，开启 Pages，选择 main 分支根目录

---

## 注意事项

- 图床链接需要替换：项目里的猫图来自私人图床，不保证长期可用，请换成自己的图片地址
- anon key 必须替换：fork 后必须换成自己 Supabase 项目的 key，否则数据会写进别人的库
- 密码是前端验证：登录密码存在代码里，仅适合私人使用
- 时区：天数基于 UTC 计算，北京时间凌晨 0 点到早上 8 点间显示可能比预期少一天

---

## 使用许可

代码部分 MIT License，自由使用和修改。

猫图版权归原图作者所有，本项目不提供图片文件，仅包含外链引用，fork 时请替换为自己有使用权的图片。
