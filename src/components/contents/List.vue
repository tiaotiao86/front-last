<template>
  <div class="fly-panel" style="margin-bottom: 0;">
    <div class="fly-panel-title fly-filter">
      <a :class="{'layui-this': status ==='' && tag === ''}" @click.prevent="search()">综合</a>
      <span class="fly-mid"></span>
      <a :class="{'layui-this': status === '0'}" @click.prevent="search(0)">未结</a>
      <span class="fly-mid"></span>
      <a :class="{'layui-this': status === '1'}" @click.prevent="search(1)">已结</a>
      <span class="fly-mid"></span>
      <a :class="{'layui-this': status === '' && tag === '精华'}" @click.prevent="search(2)">精华</a>
      <span class="fly-filter-right layui-hide-xs">
        <a :class="{'layui-this': sort === 'created'}" @click.prevent="search(3)">按最新</a>
        <span class="fly-mid"></span>
        <a :class="{'layui-this': sort === 'answer'}" @click.prevent="search(4)">按热议</a>
      </span>
    </div>
    <list-item :lists="lists" :isEnd="isEnd" @nextpage="nextPage()"></list-item>
  </div>
</template>

<script>
import { getList } from '@/api/content'
import ListItem from './ListItem'
export default {
  name: 'list',
  data () {
    return {
      status: '',
      tag: '',
      sort: 'created',
      page: 0,
      limit: 20,
      catalog: '',
      isEnd: false,
      isRepeat: false,
      lists: [
        // {
        //   uid: {
        //     name: 'imooc',
        //     isVip: 1
        //   },
        //   title: '大前端课程',
        //   content: '',
        //   created: '2019-10-01 01:00:00',
        //   catalog: 'ask',
        //   fav: 40,
        //   isEnd: 0,
        //   reads: 10,
        //   answer: 0,
        //   status: 0,
        //   isTop: 0,
        //   tags: [
        //     {
        //       name: '精华',
        //       class: 'layui-bg-red'
        //     },
        //     {
        //       name: '热门',
        //       class: 'layui-bg-blue'
        //     }
        //   ]
        // },
        // {
        //   uid: {
        //     name: 'imooc',
        //     isVip: 1
        //   },
        //   title: '大前端课程',
        //   content: '',
        //   created: '2019-10-01 01:00:00',
        //   catalog: 'ask',
        //   fav: 40,
        //   isEnd: 0,
        //   reads: 10,
        //   answer: 0,
        //   status: 0,
        //   isTop: 0,
        //   tags: [
        //     {
        //       name: '精华',
        //       class: 'layui-bg-red'
        //     },
        //     {
        //       name: '热门',
        //       class: 'layui-bg-blue'
        //     }
        //   ]
        // },
        // {
        //   uid: {
        //     name: 'imooc',
        //     isVip: 1
        //   },
        //   title: '大前端课程',
        //   content: '',
        //   created: '2019-10-01 01:00:00',
        //   catalog: 'ask',
        //   fav: 40,
        //   isEnd: 0,
        //   reads: 10,
        //   answer: 0,
        //   status: 0,
        //   isTop: 0,
        //   tags: [
        //     {
        //       name: '精华',
        //       class: 'layui-bg-red'
        //     },
        //     {
        //       name: '热门',
        //       class: 'layui-bg-blue'
        //     }
        //   ]
        // }
      ]
    }
  },
  components: {
    ListItem
  },
  mounted () {
    this._getLists()
  },
  methods: {
    _getLists () {
      if (this.isRepeat) return
      if (this.isEnd) return
      this.isRepeat = true
      let options = {
        catalog: this.catalog,
        isTop: 0,
        page: this.page,
        limit: this.limit,
        sort: this.sort,
        tag: this.tag,
        status: this.status
      }
      getList(options).then((res) => {
        this.isRepeat = false
        console.log(res)
        // 对于异常的判断，res.code 非200，我们给用户一个提示
        // 判断是否lists长度为0，如果为零即可以直接赋值
        // 当Lists长度不为0，后面请求的数据，加入到Lists里面来
        if (res.code === 200) {
          // 判断res.data的长度，如果小于20条，则是最后页
          if (res.data.length < this.limit) {
            this.isEnd = true
          }
          // 加入一个请求锁，防止用户多次点击，等待数据返回后，再打开
          if (this.lists.length === 0) {
            this.lists = res.data
          } else {
            this.lists = this.lists.concat(res.data)
          }
        }
      }).catch((err) => {
        if (err) {
          this.$alert(err.msg)
        }
      })
    },
    nextPage () {
      this.page++
      this._getLists()
    },
    search (val) {
      switch (val) {
        // 未结贴
        case 0:
          this.status = '0'
          this.tag = ''
          break
        // 已结贴
        case 1:
          this.status = '1'
          this.tag = ''
          break
        // 查询"精华"标签
        case 2:
          this.status = ''
          this.tag = '精华'
          break
        // 按照时间去查询
        case 3:
          this.sort = 'created'
          break
        // 按照评论数去查询
        case 4:
          this.sort = 'answer'
          break
        // 综合查询
        default:
          this.status = ''
          this.tag = ''
      }
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
