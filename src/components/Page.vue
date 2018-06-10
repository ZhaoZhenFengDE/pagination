<template>
  <div class="pager">
    <div class="number">
      <label for="showNum">每页显示
        <select name="showNum" id="showNum" class="form-control" v-model="pageSize" @change="changePageSize">
          <option value="15">15</option>
          <option value="30">30</option>
          <option value="45">45</option>
        </select>
      条</label>
    </div>
    <div class="page">
      <button class="btn" :disabled="this.current === 1" @click="prePage"> 上一页 </button>
      <span v-if="pageNo !== 1" :class="['page-index', {active: 1 == current}]" @click="goPage(1)"> 1 </span>
      <span v-if="preClipped" class="page-index"> ... </span>
      <span v-for="index in pages" :class="['page-index', {active: index == current}]" :key="index" @click="goPage(index)">{{ index }}</span>
      <span v-if="backClipped" class="page-index"> ... </span>
      <span :class="['page-index', {active: pageNo == current}]" @click="goPage(pageNo)">{{ pageNo }}</span>
      <button class="btn" :disabled="this.current === pageNo" @click="nextPage"> 下一页 </button>
    </div>
  </div>
</template>

<style>
.page-index {
  display: inline-block;
  margin-left: 10px;
  width: 35px;
  height: 30px;
  background-color: #fff;
  cursor: pointer;
  color: #000;
}
.active {
  color: #ffffff;
  background-color: #0bbe06;
}
</style>

<script>
export default {
  props: {
    current: {
      type: Number,
      default: 1
    },
    pageNo: {
      type: Number,
      default: 1
    }
  },
  data () {
    return {
      pageSize: 15,
      backClipped: false,
      preClipped: false
    }
  },
  methods: {
    goPage (page) {
      if (page !== this.current) {
        this.$emit('changeCurrent', page)
      }
    },
    prePage () {
      this.$emit('changeCurrent', this.current - 1)
    },
    nextPage () {
      this.$emit('changeCurrent', this.current + 1)
    },
    showPreClipped () {
      if (this.current > 4) {
        this.preClipped = true
      } else {
        this.preClipped = false
      }
    },
    showBackClipped: function () {
      if (this.current <= (this.pageNo - 4)) {
        this.backClipped = true
      } else {
        this.backClipped = false
      }
    },
    changePageSize: function (e) {
      this.$emit('changePageSize', this.pageSize)
    }
  },
  watch: {
    'current': ['showBackClipped', 'showPreClipped']
  },
  computed: {
    pages () {
      console.log(this.pageNo)
      let ret = []
      if (this.current > 4) {
        ret.push(this.current - 2)
        ret.push(this.current - 1)
      } else {
        for (let i = 2; i < this.current; i++) {
          ret.push(i)
        }
      }
      if (this.current !== this.pageNo && this.current !== 1) {
        ret.push(this.current)
      }
      if (this.current < this.pageNo - 3) {
        let i
        switch (this.current) {
          case 1:
            i = 5
            break
          case 2:
            i = 4
            break
          default:
            i = 3
            break
        }
        for (let j = 1; j < i; j++) {
          ret.push(this.current + j)
        }
      } else {
        for (let i = (this.current + 1); i < this.pageNo; i++) {
          ret.push(i)
        }
      }
      return ret
    }
  }
}
</script>
