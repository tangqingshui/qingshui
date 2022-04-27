---
title: vue3在组件上使用 v-model
date: 2022-04-26
tags:
 - vue
categories: 
 - 前端小笔记
---

## 自定义事件也可以用于创建支持 v-model 的自定义输入组件。记住：
```html
<input v-model="searchText" />
```
## 等价于：
```html
<input :value="searchText" @input="searchText = $event.target.value" />
```
## 当用在组件上时，v-model 则会这样：
```html
<custom-input
  :model-value="searchText"
  @update:model-value="searchText = $event">
</custom-input>
```
## 为了让它正常工作，这个组件内的代码必须这样写：
```html
app.component('custom-input', {
  props: ['modelValue'],
  emits: ['update:modelValue'],
  template: `
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    >
  `
})
```
## 也可以这样写
```html
app.component('custom-input', {
  props: ['modelValue'],
  emits: ['update:modelValue'],
  template: `
    <input v-model="value">
  `,
  computed: {
    value: {
      get() {
        return this.modelValue
      },
      set(value) { 
        this.$emit('update:modelValue', value)
      }
    }
  }
})
```
## 当组件内有多个input框时可以这样写：
父组件
```vue
<template>
  <myInput v-model:text="text" v-model:title="title" />
</template>

<script setup>
import { ref } from 'vue'
import myInput from './mInput'

const text = ref('')
const title = ref('1')
</script>
```
子组件
```vue
<template>
  <div>
    <el-input v-model="text" v-bind="$attrs" ref="eInput" />
     <el-input v-model="title"/>
  </div>
</template>

<script setup>
import {ref, computed, defineExpose, defineProps,defineEmits, toRefs} from 'vue'
const props = defineProps({
  text: {
    type:String
  },
  title: {
    type:String
  }
})
// 这里为了不使变量名重复多做了一层解构
const { text:text1,title:title1 } = toRefs(props)
const emit = defineEmits(['update:text','update:title'])
const text = computed({
  get: () => text1.value,
  set: val => {
    emit('update:text',val)
  }
})
const title = computed({
  get: () => title1.value,
  set: val => {
    emit('update:title',val)
  }
})
</script>
```
由此可见本质上v-model就是一个语法糖，value绑定一个变量，然后监听input事件，重新给value赋值
