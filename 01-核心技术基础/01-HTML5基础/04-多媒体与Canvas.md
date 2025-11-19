# 多媒体与 Canvas

## 音频与视频

```html
<audio controls src="music.mp3">您的浏览器不支持 audio 标签。</audio>

<video controls width="480" poster="cover.jpg">
  <source src="video.webm" type="video/webm" />
  <source src="video.mp4" type="video/mp4" />
  <track
    kind="captions"
    src="subtitles.vtt"
    srclang="zh"
    label="中文字幕"
    default
  />
  您的浏览器不支持 video 标签。
</video>
```

要点：

- `controls/loop/muted/autoplay`。
- `<track>` 字幕与可访问性。
- 提供多编码以适配不同浏览器。

## Canvas 基础

```html
<canvas id="c" width="300" height="150"></canvas>
<script>
  const ctx = document.getElementById("c").getContext("2d");
  // 矩形
  ctx.fillStyle = "#22c55e";
  ctx.fillRect(10, 10, 100, 60);
  // 线条
  ctx.beginPath();
  ctx.moveTo(10, 90);
  ctx.lineTo(110, 140);
  ctx.strokeStyle = "#2563eb";
  ctx.lineWidth = 4;
  ctx.stroke();
  // 文本
  ctx.font = "16px system-ui";
  ctx.fillStyle = "#000";
  ctx.fillText("Hello Canvas", 130, 40);
</script>
```

## 高 DPI 适配

```js
function setupHiDpiCanvas(canvas) {
  const ratio = window.devicePixelRatio || 1;
  const { width, height } = canvas.getBoundingClientRect();
  canvas.width = Math.round(width * ratio);
  canvas.height = Math.round(height * ratio);
  const ctx = canvas.getContext("2d");
  ctx.scale(ratio, ratio);
  return ctx;
}
```

## 练习

- 用 Canvas 绘制一个进度环与动画加载。\*\*\*

---

## 知识拓展

- `<video>` 播放控制与画中画（PiP）

```html
<video id="v" controls src="intro.mp4" playsinline muted></video>
<button id="pip">画中画</button>
<script>
  pip.onclick = async () => {
    try {
      if (document.pictureInPictureElement) {
        await document.exitPictureInPicture();
      } else {
        await v.requestPictureInPicture();
      }
    } catch (e) {
      console.error(e);
    }
  };
</script>
```

- 从视频帧抓取缩略图（Canvas）

```html
<video id="video" src="intro.mp4" crossorigin="anonymous"></video>
<canvas id="thumb" width="320" height="180"></canvas>
<script>
  const video = document.getElementById("video");
  const canvas = document.getElementById("thumb");
  const ctx = canvas.getContext("2d");
  video.addEventListener("loadeddata", () => {
    video.currentTime = 2; // 定位到第 2 秒
  });
  video.addEventListener("seeked", () => {
    ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
  });
</script>
```

- Canvas 导出图片与离屏渲染

```js
const url = canvas.toDataURL("image/png"); // 下载/上传
// 离屏：大计算量绘制可用 OffscreenCanvas（部分浏览器）
```
