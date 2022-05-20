# 替换 iOS Kindle 的默认字体
Rplace the iOS Kindle APP's font.
> 引自：https://bookfere.com/post/959.html

## 一些字体
- 仓耳今楷预览链接（官网）：http://www.tsanger.cn/category/21
- 03 字重下载链接（官网）：http://www.tsanger.cn/download/仓耳今楷05-W03.ttf
- 04 字重下载链接（官网）：http://www.tsanger.cn/download/仓耳今楷05-W04.ttf
- 05 字重下载链接（官网）：http://www.tsanger.cn/download/仓耳今楷05-W05.ttf

![仓耳今楷](https://bookfere.com/wp-content/uploads/2022/03/kindle-change-font-on-all-platform-1.png)

## 一、为 Kindle App「iOS」更换自定义字体
由于 iOS 系统限制，无法像 Kindle 设备那样使用自定义字体的方法替换 Kindle APP 下载字体，因此需要配合 iOS 上的网络调试工具重写下载字体的网络地址，此处以 Surge 为例展示操作方法，配置和信任证书的教程自行网络搜索，其他工具同理。抓包得到 Kindle 下载字体的连接为：

- 楷体：https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STKaitiSC.ttf?XXXXXXXXXXXXXX
- 黑体：https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STHeitiSC.ttf?XXXXXXXXXXXXXX
- 宋体：https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STSongSC.ttf?XXXXXXXXXXXXXX
- 圆体：https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STYuanMedium-2018-02-16.ttf?XXXXXXXXXXXXXX

## 二、修改字体信息
**先从第一步获取需要修改字体的下载链接，下载字体，以用来查看字体信息。**

以宋体为例，浏览器打开抓包链接下载宋体字体文件，使用在线字体编辑器：

> FontEditor：https://kekee000.github.io/fonteditor/index.html

**注意事项：①留够内存 ②Edge 一直崩，推荐 Firefox 流览器**

- 打开「仓耳今楷05-W03.ttf」，并在【设置 → 字体信息】中将所有信息替换为“STSongSC.ttf”中的信息。
- 然后导出 TTF 文件，重命名**下载的TTF**为「STSongSC.ttf」
- 上传至自己的 Github 或者其他可以直接访问的地址。

### 三、网络工具的配置
最后得到 302 重写规则，创建如下所示的 Surge 模块「注意，一定要把代码里的字体地址 `https://example.com/STSongSC.ttf` 替换成你上传字体后得到的真实可用的地址」：

```bash
#!name=Kindle Fonts Customize
#!desc=替换 Kindel iOS APP 默认字体

[URL Rewrite]
# > 宋体
^https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STSongSC.ttf.+ https://YourFontURLs/STSongSC.ttf 302
# > 楷体
# ^https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STKaitiSC.ttf.+
# > 黑体
# ^https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STHeitiSC.ttf.+
# > 圆体
# ^https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STYuanMedium-2018-02-16.ttf.+

[MITM]

hostname = %APPEND% s3.cn-north-1.amazonaws.com.cn
```

## 四、替换字体
- Surge 新建本地模块，添加上述模块内容并保存。
- 将 Kindle 下载的字体删除并重启应用。
- Surge 开启字体替换模块。
- Kindle 下载对应的字体。
- 成功后 Surge 关闭字体替换模块。
