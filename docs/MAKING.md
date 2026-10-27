# 制作步骤

## 1. 内容与节奏

核对 [Lucian UX/UI Prompt](https://lucianlabs.dev/uxui-prompt/) 的 228、314、323、325、363。完整来源和逐秒分镜见 [treatment.md](../film/treatment.md)。

|时间|内容|
|---|---|
|0–2 秒|五种控件总览与钩子|
|2–5 秒|流光边框：功能卡片|
|5–8 秒|变形光标：品牌官网|
|8–11 秒|照片拖尾：创意作品集|
|11–14 秒|代码透视镜：组件教学|
|14–17 秒|点阵波纹：科技首页|
|17–20 秒|总结：场景 + 动作 + 反馈|

## 2. 影片

`film/index.html` 暴露确定性的 `draw(t)`，本地字体加载后开始渲染。标题、控件和提示词为独立 Bento 面板。每个控件编号与身份保持不变，放大后回到缩略卡。

先检查九个关键时间点，完成两轮修改：调整代码镜片落点和示例文字间距；整理照片展开区域，匹配光标到位后的形态说明。

## 3. 配乐与导出

`make_audio.py` 使用 Python / numpy 合成 120 BPM、2/4 拍配乐，首拍 0 秒，每小节 1 秒，全部章节落在小节线。18 秒回到主和弦，末尾 0.8 秒淡出。音效跟随操作时刻。

```sh
cd film
npm install
python3 make_audio.py
npm run stills
npm run render
sh encode.sh
```

需要 Chrome、Node.js、Python 3 + numpy 和 FFmpeg。设置 `CHROME_PATH` 可切换 Chrome 路径。渲染 1,200 张 JPG，编码 H.264 Main / yuv420p + AAC，并开启 faststart 和每秒关键帧。更多信息见 [影片说明](../film/README.md) 和 [影片验收](../film/QA.md)。

## 4. 可交互网页

网页沿用影片控件配色和视觉素材，改由实际指针、触屏、键盘驱动。

- 流光：统一指针坐标转成各卡片局部坐标，径向渐变照亮相邻边缘。
- 变形：根据命中目标改变装饰光标；装饰层不拦截点击。
- 照片：按移动距离生成 DOM 照片，限制数量，取消与结束时清理动画；点击展开五张。
- 透视：同位置 UI / 代码双层，圆形 clip-path 跟随指针；点击切换全卡代码。
- 波纹：Canvas 按距离、时间计算波前，叠加后衰减；无波纹时停止绘制。

触屏提供点按反馈；切换控件、重置和页面隐藏时清理临时动画；减少动态模式降低运动。

## 5. 发布

将仓库提交到 main，在 Settings → Pages 中选择 Deploy from a branch → main → / (root)。根目录 `.nojekyll` 保持静态文件原样发布。

发布地址：[vibe-MoveBeSeen](https://jdb156158.github.io/vibe-MoveBeSeen/)。上线后再次检查交互、媒体资源、完整播放及跳转。
