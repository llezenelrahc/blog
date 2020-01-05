---
title: how-deep-works
date: 2020-01-05 12:57:53
tags:
---


> 想象这么一个场景，当前你已经有一个按钮的组件，
> 当组件存在于某个容器 A 中时，按钮需要表现出的 margin 是 10px，
> 当组件存在于某个容器 B 中时，按钮需要表现出的 margin 是 20px。

```html
// Button.vue
<template>
  <div class="btn">{{text}}</div>
</template>
<style scope>
  .btn {
    border: 1px solid red;
  }
</style>
<script>
  export default {
    props: {
      text: {
        type: String,
        default: "i'm a button text",
      },
    },
  };
</script>
```

```html
// ContainerA.vue
<template>
  <div class="containerA">
    <button text="sure" />
    <button text="cancel" />
  </div>
</template>
<style scope>
  .containerA {
    .btn {
      margin: 10px;
    }
  }
</style>
<script>
  import Button from 'some/path/to/Button.vue';
  export default {
    components: { Button },
  };
</script>
```

```diff
// ContainerB.vue
<template>
-  <div class="containerB">
+  <div class="containerB">
    <button text="sure" />
    <button text="cancel" />
  </div>
</template>
<style scope>
-  .containerA {
+  .containerA {
    .btn {
-     margin: 10px;
+     margin: 20px;
    }
  }
</style>
<script>
  import Button from 'some/path/to/Button.vue';
  export default {
    components: { Button },
  };
</script>
```

> 一切看起来那么的自然？

> 看一下如果 button 的实现修改了？

```diff
// Button.vue
<template>
+  <div>
    <div class="btn">{{text}}</div>
+  </div>
</template>
<style scope>
  .btn {
    border: 1px solid red;
  }
</style>
<script>
  export default {
    props: {
      text: {
        type: String,
        default: "i'm a button text",
      },
    },
  };
</script>
```

> 此时两个margin都不会应用上？
> why....

>  [文档这么说](https://vue-loader.vuejs.org/zh/guide/scoped-css.html#%E5%AD%90%E7%BB%84%E4%BB%B6%E7%9A%84%E6%A0%B9%E5%85%83%E7%B4%A0) => [代码中这么说](https://github.com/vuejs/vue/blob/050bb33f9b02589357c037623ea8cbf8ff13555b/src/core/vdom/patch.js#L282)

> heheh