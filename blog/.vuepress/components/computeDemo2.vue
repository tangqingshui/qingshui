<template>
  <div>
    <compute-demo1>
      <template #demo="{data}">
        <ul>
          <li v-for="(item, index) in filterUsr(data)" :key="index">{{item}}</li>
        </ul>
        <button @click="randomHiddenUser(data)">随机隐藏用户</button>
        <button @click="cancelHiddenUser">撤销隐藏</button>
      </template>
    </compute-demo1>
  </div>
</template>

<script>
import computeDemo1 from './computeDemo1.vue'
export default {
  components: {
    computeDemo1
  },
  data () {
    return {
      user: [
        {
          name: 'tomi',
          age: 24,
          visble: false
        },
        {
          name: 'jimi',
          age: 15
        },
        {
          name: 'lami',
          age: 89
        },
        {
          name: 'momi',
          age: 33
        }
      ],
      userQueue: []
    }
  },
  computed: {
    filterUsr () {
      return this.filterData
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
    randomHiddenUser (data) {
      const hiddenUserindex = this.getUserIndex(data)
      const leg = hiddenUserindex.length
      console.log(hiddenUserindex)
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
    getUserIndex (data) {
      const arr = []
      this.user.forEach((item, index) => {
        if (item.visble !== false && data.age < item.age) {
          arr.push(index)
        }
      })
      return arr
    },
    filterData (data) {
      return this.user.filter(item => {
        if (item.visble !== false && data.age < item.age) {
          return true
        }
      }).map(item => item.name + data.age)
    }
  }
}
</script>
