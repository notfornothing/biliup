# 要留下的配置

录和传用 biliup 自己的功能。下面只记和程序默认不一样、而且要留的项。其余全局项（分段大小、线路 `AUTO`、下播后再等 300 秒、线程数等）保持默认。

## 留下

- 直播流协议 `hls_fmp4`。默认是 FLV，这条是为了少断流。
- 文件名和投稿标题：`{streamer}｜%Y-%m-%d %H-%M-%S｜{title}`（全角竖线）。默认没有这套命名。
- 投稿通道 `web`。空着会走 App，会被 `21566` 拦住。
- 投稿模板：简介两行 `主播:{streamer}`、`地址:{url}`；自制；仅自己可见。默认投稿类型是转载。
- 录完删除本地文件（后处理 `rm`）。默认不删。

## 画质

用默认原画，不写死。曾经设成 `250`（超清），已经清掉。

## 直播间

测试房间 `410175` 已删。本机现在是从服务器搬来的 6 个房间，都挂在同一个投稿模板上，录完 `rm`：

- uglybeast `https://live.bilibili.com/27176522`
- 叶子长青K `https://www.douyu.com/246195`
- 雨露柘榴S `https://live.bilibili.com/1846982155`
- 皮卡邱 `https://live.douyin.com/kk052525`
- Dota2-AhJit阿捷 `https://live.douyin.com/44266904274`
- Dota2卡尔卡滨kb `https://live.douyin.com/380362637`
