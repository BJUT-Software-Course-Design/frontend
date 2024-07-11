<template>
  <Row type="flex" :gutter="18">
    <Col :span="24">
      <Panel shadow>
        <div slot="title">学生列表</div>
        <div slot="extra">
          <ul class="filter">
            <li>
              <!-- <Input v-model="query.keyword"
                     @keyup.enter="filterByKeyword"
                     placeholder="输入姓名搜索学生"
                     icon="ios-search-strong"/> -->
              <Button type="info" @click="onReset">
                <!--<Icon type="refresh"></Icon>-->
                添加学生
              </Button> 
            </li>
            <li>
              <Button type="info" @click="onReset">
                <!--<Icon type="refresh"></Icon>-->
                发布作业
              </Button> 
            </li>
          </ul>
        </div>
        <Table style="width: 100%; font-size: 16px;"
               :columns="homeworkTableColumns"
               :data="homeworkList"
               :loading="loading.homework"
               disabled-hover></Table>
      </Panel>
      <Pagination
        :total="total"
        :page-size.sync="query.limit"
        @on-change="handlePageChange"
        :current.sync="query.page"
        :show-sizer="true"></Pagination>
    </Col>
  </Row>
</template>

<script>
  import { mapGetters } from 'vuex'
  import api from '@oj/api'
  import utils from '@/utils/utils'
  import { ProblemMixin } from '@oj/components/mixins'
  import Pagination from '@oj/components/Pagination'

export default {
  name: 'HomeworkList',
  mixins: [ProblemMixin],
  components: {
    Pagination
  },
  data () {
    return {
      homeworkTableColumns: [
        {
          title: '学号',
          key: '_id',
          width: 100,
          align: 'center',
          render: (h, params) => {
            //return h('Button', {
            //   props: {
            //     type: 'text',
            //     size: 'large'
            //   },
            //   on: {
            //     click: () => {
            //       this.$router.push({ name: 'homework-details', params: { homeworkID: params.row._id } })
            //     }
            //   }
            // }, params.row._id)
            return h('span', params.row._id); // 直接返回文本内容而不是按钮，更改为编号不跳转
          }
        },
        {
          title: '学生名称',
          render: (h, params) => {
            return h('Button', {
               props: {
                 type: 'text',
            size: 'large'
               },
               on: {
                 click: () => {
                   this.$router.push({ name: 'homework-details', params: { homeworkID: params.row.name } })
                 }
              }
             }, params.row.name)
            
        },
      }
        // {
        //   title: '截止时间',
        //   key: 'deadline'
        // },
        // {
        //   title: '作业状态',
        //   key: 'status'
        // }
      ],
      homeworkList: [],
      loading: {
        homework: true
      },
      total: 0,
      query: {
        keyword: '',
        page: 1,
        limit: 10
      }
    }
  },
  mounted () {
    this.init()
  },
  methods: {
    init () {
      this.homeworkList = [
        { _id: '1', name: '刘备', deadline: '2024-07-10', status: '未完成' },
        { _id: '2', name: '关羽', deadline: '2024-07-15', status: '已完成' },
        { _id: '3', name: '张飞', deadline: '2024-07-20', status: '未完成' },
        // 添加更多的虚拟数据
      ];
      this.getHomeworkList()
    },
    getHomeworkList () {
      let offset = (this.query.page - 1) * this.query.limit
      this.loading.homework = true
      api.getHomeworkList(offset, this.query.limit, this.query).then(res => {
        this.loading.homework = false
        this.total = res.data.total
        this.homeworkList = res.data.results
      }).catch(err => {
        this.loading.homework = false
        console.error('Failed to fetch homework list: ', err)
      })
    },
    filterByKeyword () {
      this.query.page = 1
      this.getHomeworkList()
    },
    onReset () {
      this.query.keyword = ''
      this.query.page = 1
      this.getHomeworkList()
    },
    handlePageChange (page) {
      this.query.page = page
      this.getHomeworkList()
    }
  },
  computed: {
    ...mapGetters(['isAuthenticated'])
  },
  watch: {
    '$route' () {
      this.init()
    }
  }
}
</script>

<style scoped lang="less">
.filter {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.filter li {
  display: inline-block;
  margin-right: 10px;
}

.filter li:last-child {
  margin-right: 0;
}

#app {
  margin: 20px;
}
</style>
