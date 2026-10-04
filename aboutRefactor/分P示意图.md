# 一个 P 怎么装满

断流切出来的小文件先留在磁盘上，用 FFmpeg 接进当前这一杯。杯子有两条满线，先碰到哪条，这一杯就是 1 个 P，马上上传。同一场直播只有一条稿：第一杯新建稿件，后面的杯子追加到这条稿上。直播还在继续，下一杯从空杯再接。

- 实线：体积，满 **30GB**
- 虚线：时长，满 **2 小时**
- 下播：杯子没满也上传。这一场剩下的小文件合成最后一个 P

```mermaid
xychart-beta
    title "一杯的两条满线（先到先上传）"
    x-axis "这一杯已经录了多久（分钟）" [0, 30, 60, 90, 120]
    y-axis "这一杯的体积（GB）" 0 --> 36
    line "体积涨上去" [0, 8, 16, 24, 30]
    line "30GB 满线" [30, 30, 30, 30, 30]
```

体积在 120 分钟碰到 30GB，这一杯就上传。时长先满 2 小时、体积还没到 30GB 时，也上传。

下播不看这两条线。例如下一杯只录了 40 分钟、0.7GB，主播下播，这一杯照样上传。

```mermaid
flowchart TD
    startNode[开始录这一杯] --> addPiece[断流小文件接进杯子]
    addPiece --> fullSize{到 30GB 了吗}
    fullSize -->|到了| uploadNow[这一杯作为一个 P 马上上传到这一场的同一条稿]
    fullSize -->|没有| fullTime{到 2 小时了吗}
    fullTime -->|到了| uploadNow
    fullTime -->|没有| stillLive{还在播吗}
    stillLive -->|在播| addPiece
    stillLive -->|下播了| uploadRest[杯子里剩下的也追加到这一场的同一条稿]
    uploadNow --> nextCup[下一杯从空杯开始]
    nextCup --> addPiece
```
