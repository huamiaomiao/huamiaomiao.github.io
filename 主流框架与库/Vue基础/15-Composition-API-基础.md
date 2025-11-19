# Composition API 基础

## setup / ref / reactive

```vue
<script setup>
import { ref, reactive } from "vue";
const count = ref(0);
const user = reactive({ name: "Ada", age: 20 });
</script>
<template>
  <button @click="count++">{{ count }}</button>
  <p>{{ user.name }} - {{ user.age }}</p>
</template>
```

## computed / watch

```js
import { computed, watch } from "vue";
const double = computed(() => count.value * 2);
watch(
  () => user.age,
  (n) => console.log("age ->", n)
);
```

## 组合逻辑（可复用）

````js
// useToggle.ts
import { ref } from 'vue';
export function useToggle(initial = false) {
  const v = ref(initial);
  const toggle = () => v.value = !v.value;
  return { v, toggle };
}
```***

````

---

## 知识拓展

- 解构响应式的陷阱与 `toRefs`

```ts
import { reactive, toRefs } from "vue";
const state = reactive({ a: 1, b: 2 });
// 直接解构会失去响应性
// const { a, b } = state; // 非响应
const { a, b } = toRefs(state); // 响应
```

- 自定义组合函数中的副作用清理

```ts
import { onMounted, onUnmounted } from "vue";
export function useInterval(cb: () => void, ms: number) {
  let id: number | undefined;
  onMounted(() => (id = window.setInterval(cb, ms)));
  onUnmounted(() => clearInterval(id));
}
```

- `shallowRef`/`shallowReactive` 适用场景（大对象 & 第三方实例）
