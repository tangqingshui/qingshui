<template>
  <div>
    <ul>
      <li v-for="(item,index) in filterUser" :key="index">{{item.name+item.age}}</li>
    </ul>
    <button @click="randomHiddenUser">随机隐藏用户</button>
    <button @click="cancelHiddenUser">撤销隐藏</button>
  </div>
</template>

<script>

export default {
  name: 'computeDemo',
  data () {
    return {
      user: [
        {
          name: 'tomi',
          age: 23
        },
        {
          name: 'jimi',
          age: 23
        },
        {
          name: 'lami',
          age: 23
        },
        {
          name: 'momi',
          age: 23
        }
      ],
      userQueue: []
    }
  },
  computed: {
    filterUser () {
      console.log(1)
      return this.user.filter(item => item.visble !== false)
    }
  },
  methods: {
    cancelHiddenUser () {
      const leg = this.userQueue.length
      if (leg === 0) {
        return
      }
      const index = this.userQueue.pop()
      this.user = this.user.map((item, i) => {
        if (index === i) {
          item.visble = true
        }
        return item
      })
    },
    randomHiddenUser () {
      const hiddenUserindex = this.getUserIndex()
      const leg = hiddenUserindex.length
      if (leg === 0) {
        return
      }
      const index = hiddenUserindex[Math.floor(Math.random() * leg)]
      this.user = this.user.map((item, i) => {
        console.log(item, i, index)
        if (index === i && item.visble !== false) {
          item.visble = false
          this.userQueue.push(i)
        }
        return item
      })
    },
    getUserIndex () {
      const arr = []
      this.user.forEach((item, index) => {
        item.visble !== false && (arr.push(index))
      })
      return arr
    }
  }
}
</script>
