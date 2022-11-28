---
title: 分享一个vue固钉组件
date: 2021-04-13 16:20:44
tags: 
  - vue
categories:
 - 前端
---
element 中没有vue固钉组件 ,而我发下ant-design-vue中做了相应的组件，总不能再element-ui的项目中再使用ant-design-vue吧，所以我抽离了组件出来。
# 代码
```js
<template>
  <div class="affix-placeholder" :style="wrapStyle">
    <div :class="{ affix: affixed }" :style="styles">
      <slot></slot>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    offset: {
      type: Number,
      default: 0,
    },
    onAffix: {
      type: Function,
      default() {},
    },
    boundary: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      affixed: false,
      styles: {},
      affixedClientHeight: 0,
      wrapStyle: {},
    };
  },
  methods: {
    getScroll(w, top) {
      let ret = w[`page${top ? "Y" : "X"}Offset`];
      const method = `scroll${top ? "Top" : "Left"}`;
      if (typeof ret !== "number") {
        const d = w.document;
        // ie6,7,8 standard mode
        ret = d.documentElement[method];
        if (typeof ret !== "number") {
          // quirks mode
          ret = d.body[method];
        }
      }
      return ret;
    },
    getOffset(element) {
      const rect = element.getBoundingClientRect();
      const body = document.body;
      const clientTop = element.clientTop || body.clientTop || 0;
      const clientLeft = element.clientLeft || body.clientLeft || 0;
      // const clientHeight = element.clientHeight || 0;
      const scrollTop = this.getScroll(window, true);
      const scrollLeft = this.getScroll(window);
      return {
        top: rect.bottom + scrollTop - clientTop - this.affixedClientHeight,
        left: rect.left + scrollLeft - clientLeft,
      };
    },
    handleScroll() {
      const scrollTop = this.getScroll(window, true) + this.offsets; // handle setting offset
      const elementOffset = this.getOffset(this.$el);
      if (!this.affixed && scrollTop > elementOffset.top) {
        this.affixed = true;
        this.styles = {
          top: `${this.offsets}px`,
          left: `${elementOffset.left}px`,
          width: `${this.$el.offsetWidth}px`,
        };
        this.onAffix(this.affixed);
      }
      // if setting boundary
      if (this.boundary && scrollTop > elementOffset.top) {
        const el = document.getElementById(this.boundary.slice(1));
        if (el) {
          const boundaryOffset = this.getOffset(el);
          if (scrollTop + this.offsets > boundaryOffset.top) {
            const top = scrollTop - boundaryOffset.top;
            this.styles.top = `-${top}px`;
          }
        }
      }
      if (this.affixed && scrollTop < elementOffset.top) {
        this.affixed = false;
        this.styles = {};
        this.onAffix(this.affixed);
      }
      if (this.affixed && this.boundary) {
        const el = document.getElementById(this.boundary.slice(1));
        if (el) {
          const boundaryOffset = this.getOffset(el);
          if (scrollTop + this.offsets <= boundaryOffset.top) {
            this.styles.top = 0;
          }
        }
      }
    },
  },
  computed: {
    offsets() {
      if (this.boundary) {
        return 0;
      }
      return this.offset;
    },
  },
  mounted() {
    this.affixedClientHeight = this.$el.children[0].clientHeight;
    this.wrapStyle = { height: `${this.affixedClientHeight}px` };
    window.addEventListener("scroll", this.handleScroll);
    window.addEventListener("resize", this.handleScroll);
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.handleScroll);
    window.removeEventListener("resize", this.handleScroll);
  },
};
</script>
<style >
.affix {
  position: fixed;
}
</style>
```
# 使用
```html
<!-- 固钉距离 80px -->
        <my-affix :offset="80">
          <!-- parentClass 指定文章内容的父元素class值 -->
          <my-directory parentClass="article-content"></my-directory>
        </my-affix>
```
参考  https://2x.antdv.com/components/affix-cn