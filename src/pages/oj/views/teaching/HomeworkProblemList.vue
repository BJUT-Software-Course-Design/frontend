<template>
    <Row type="flex" :gutter="18">
      <Col :span="24">
        <Panel shadow>
          <div slot="title">作业题目选择</div>
          <Table style="width: 100%; font-size: 16px;"
                 :columns="problemTableColumns"
                 :data="problemList"
                 :loading="loading.problems"
                 disabled-hover></Table>
        </Panel>
      </Col>
    </Row>
  </template>
  
  <script>
  import { mapGetters } from 'vuex'
  import api from '@oj/api'
  import { ProblemMixin } from '@oj/components/mixins'
  
  export default {
    name: 'HomeworkProblemList',
    mixins: [ProblemMixin],
    data () {
      return {
        problemTableColumns: [
          {
            title: '题目编号',
            key: '_id',
            width: 100,
            align: 'center',
            render: (h, params) => {
              return h('Button', {
                props: {
                  type: 'text',
                  size: 'large'
                },
                on: {
                  click: () => {
                    this.$router.push({ name: 'problem-details', params: { problemID: params.row._id } })
                  }
                }
              }, params.row._id)
            }
          },
          {
            title: '题目名称',
            key: 'name',
            render: (h, params) => {
              return h('Button', {
                props: {
                  type: 'text',
                  size: 'large'
                },
                on: {
                  click: () => {
                    this.$router.push({ name: 'problem-details', params: { problemID: params.row._id } })
                  }
                }
              }, params.row.name)
            }
          },
          {
            title: '题目状态',
            key: 'status'
          }
        ],
        problemList: [],
        loading: {
          problems: true
        }
      }
    },
    mounted () {
      this.init()
    },
    methods: {
      init () {
        this.getProblemList()
      },
      getProblemList () {
        // Replace with your API call to fetch problem list for this homework
        // Example:
        // api.getProblemListForHomework(this.$route.params.homeworkID)
        this.loading.problems = true
        // Simulated data
        setTimeout(() => {
          this.loading.problems = false
          this.problemList = [
            { _id: '1', name: 'Problem 1', status: 'Pending' },
            { _id: '2', name: 'Problem 2', status: 'Completed' },
            { _id: '3', name: 'Problem 3', status: 'Pending' }
          ]
        }, 1000) // Simulated delay for demo purposes
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
  #app {
    margin: 20px;
  }
  </style>
  