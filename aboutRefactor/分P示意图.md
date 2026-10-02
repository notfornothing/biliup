# 一个 P 怎么装满

断流切出来的小文件先留在磁盘上，用 FFmpeg 接进当前这一杯。杯子有两条满线，先碰到哪条，这一杯就是 1 个 P，马上上传。直播还在继续，下一杯从空杯再接。

- 实线：体积，满 **2GB**
- 虚线：时长，满 **2 小时**
- 下播：杯子没满也上传。这一场剩下的小文件合成最后一个 P

```mermaid
xychart-beta
    title "一杯的两条满线（先到先上传）"
    x-axis "这一杯已经录了多久（分钟）" [0, 30, 60, 90, 120]
    y-axis "这一杯的体积（GB）" 0 --> 2.5
    line "体积涨上去" [0, 0.6, 1.3, 2.0, 2.0]
    line "2GB 满线" [2, 2, 2, 2, 2]
```

体积在 90 分钟碰到 2GB，还没到 2 小时，这一杯就上传。2 小时那条是另一条满线：体积涨得慢时，录满 120 分钟也上传。

下播不看这两条线。例如下一杯只录了 40 分钟、0.7GB，主播下播，这一杯照样上传。

```mermaid
flowchart TD
    startNode[开始录这一杯] --> addPiece[断流小文件接进杯子]
    addPiece --> fullSize{到 2GB 了吗}
    fullSize -->|到了| uploadNow[这一杯作为一个 P 马上上传]
    fullSize -->|没有| fullTime{到 2 小时了吗}
    fullTime -->|到了| uploadNow
    fullTime -->|没有| stillLive{还在播吗}
    stillLive -->|在播| addPiece
    stillLive -->|下播了| uploadRest[杯子里剩下的也作为一个 P 上传]
    uploadNow --> nextCup[下一杯从空杯开始]
    nextCup --> addPiece
```
