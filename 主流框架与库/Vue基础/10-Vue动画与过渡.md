# Vue 动画与过渡

> 目标：学会用 `<transition>`/`<transition-group>` 实现进入/离开/列表动画，并注意性能。

## 单元素过渡

```html
<transition name="fade">
  <p v-if="show">Hello</p>
</transition>
<style>
  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.2s ease;
  }
</style>
```

## 列表过渡

```html
<transition-group name="list" tag="ul">
  <li v-for="item in items" :key="item.id">{{ item.text }}</li>
</transition-group>
<style>
  .list-enter-from,
  .list-leave-to {
    opacity: 0;
    transform: translateY(6px);
  }
  .list-enter-active,
  .list-leave-active {
    transition: all 0.18s ease;
  }
  .list-move {
    transition: transform 0.18s ease;
  } /* 位移过渡 */
</style>
```

## 自定义过渡钩子

```html
<transition @enter="onEnter" @leave="onLeave">...</transition>
```

## 与第三方动画库结合

可在 `enter-active-class`/`leave-active-class` 中指定类名，交由 Animate.css/GSAP 等处理。

## 性能建议

- 优先使用 `transform`/`opacity` 动画
- 控制并发数量与层级，避免大面积重排

## 练习建议

- 列表的增删带过渡；弹层的淡入淡出；路由切换淡入
