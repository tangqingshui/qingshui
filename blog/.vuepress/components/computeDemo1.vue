<template>
  <div>
    <slot name="demo" :data="user"></slot>
  </div>
</template>

<script>

export default {
  data () {
    return {
      user: {
        age: 23
      }
    }
  }
}
</script>
