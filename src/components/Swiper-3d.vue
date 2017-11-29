<template>
  <div class="swiper-box">
    <div class="swiper-list">
      <div v-for="(item, index) in items" :key="index" class="item" :style="{width:item.width+'px',height:item.height+'px',left:item.left+'px',bottom:item.bottom+'px','z-index':item.zIndex,opacity:item.opacity,'background-color':rlist[index].color}">
          <span :style="{'line-height':item.height+'px'}">{{rlist[index].content}}</span>
      </div>
    </div>
    <div class="swiper-point">
      <div class="point" v-for="(item, index) in items" :key="index" :class="globalKey==index&&'on'"></div>
    </div>
  </div>
</template>
<script>
  export default {
    name: 'swiper-3d',
    props: ['list', 'allWidth', 'allHeight', 'curHeight', 'curWidth', 'scale'],
    data () {
      let items = []
      let rlist = this.copyArr(this.list)
      let level = Math.floor(this.list.length / 2)
      if (this.list.length % 2 === 0) {
        let center = this.list[0]
        rlist.push(Object.assign({}, center))
      }
      let lefts = rlist.slice(0, level)
      let rights = rlist.slice(level)
      let that = this
      let leftGap = (this.allWidth - this.curWidth) / 2
      let gap = leftGap / level
      lefts.forEach((e, i) => {
        let obj = {}
        // console.log('left', e.content)
        obj.content = e.content
        // obj.left = i * gap;
        obj.left = i * gap - that.curWidth + leftGap + gap * 2.5
        obj.zIndex = i + 1
        obj.opacity = 1 / (level + 1 - i)
        obj.width = that.curWidth * Math.pow(that.scale, level - i)
        obj.height = that.curHeight * Math.pow(that.scale, level - i)
        obj.bottom = (that.allHeight - obj.height) / 2
        obj.key = items.length
        items.push(obj)
      })
      rights.forEach((e, i) => {
        let obj = {}
        // console.log('right', e.content)
        obj.content = e.content
        obj.width = that.curWidth * Math.pow(that.scale, i)
        obj.height = that.curHeight * Math.pow(that.scale, i)
        // obj.left = that.allWidth - (level - i) * gap - obj.width
        obj.left = leftGap + i * (that.curWidth + (level - i) * gap - (gap / 2))
        obj.zIndex = level - i + 1
        obj.opacity = 1 / (i + 1)
        obj.bottom = (that.allHeight - obj.height) / 2
        obj.key = items.length
        items.push(obj)
      })
      return {
        items: items,
        rlist: rlist,
        timer: null,
        dir: 'right',
        globalKey: level,
        level: level
      }
    },
    created () {
      this.setTimer()
    },
    methods: {
      setTimer () {
        let that = this
        this.clearTimer()
        function cb () {
          that.timer = setTimeout(() => {
            if (that.dir === 'right') {
              let pop = that.items.shift()
              that.items.push(pop)
              that.globalKey = that.items[that.level].key
            } else {
              let pop = that.items.pop()
              that.items.unshift(pop)
              that.globalKey = that.items[that.level].key
            }
            cb()
          }, 2000)
        }
        cb()
      },
      clearTimer () {
        if (this.timer) {
          clearTimeout(this.timer)
        }
      },
      copyArr (arr) {
        return arr.map((e) => {
          if (typeof e === 'object') {
            return Object.assign({}, e)
          } else {
            return e
          }
        })
      }
    }
  }
</script>
<style scoped>
    .swiper-box{
        width:100%;
        overflow: hidden;
        opacity: 0.7;
        background-color: #B9E1FE;
    }
    .swiper-list{
        width:100%;
        height:7.4rem;
        position:relative;
        margin:0 auto;
    }
    .item{
        box-sizing:border-box;
        position:absolute;
        display:block;
        background-color:blue;
        color:#fff;
        text-align:center;
        transition: all .8s ease;
        border-radius:5px;
        box-shadow: 0px 0px 10px #999;
    }
    .item span{
        transition:inherit;
        font-size:60px;
    }
    .swiper-point{
        width: 100%;
        height: 0.8rem;
        line-height: 0.3rem;
        text-align: center;
        margin-top: -0.1rem;
    }
    .swiper-point .point{
        display:inline-block;
        width:0.2rem;
        height: 0.2rem;
        border-radius: 50%;
        opacity: 0.7;
        background-color: #FFFFFF;
        margin-right:0.3rem;
    }
    .swiper-point .point.on{
        background-color:#F99A27;
    }
</style>
