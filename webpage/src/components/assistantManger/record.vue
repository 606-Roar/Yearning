<style lang="less">
@import "../../styles/common.less";
@import "../order/components/table.less";
</style>
<template>
  <div>
    <Row>
      <Card>
        <p slot="title">
          <Icon type="md-send"></Icon>历史工单执行记录
        </p>
        <Row>
          <Col span="24">
            <Table
              border
              :columns="tabcolumns"
              :data="TableDataNew"
              class="tabletop"
              style="background: #5cadff"
            ></Table>
            <br>
            <Page
              :total="this.pagenumber"
              show-elevator
              @on-change="splicpage"
              :page-size="20"
              ref="page"
            ></Page>
          </Col>
        </Row>
      </Card>
    </Row>
  </div>
</template>
<script>
import axios from 'axios'
//
export default {
  name: 'Record',
  data () {
    return {
      tabcolumns: [
        {
          title: '工单',
          key: 'work_id'
        },
        {
          title: '工单说明',
          key: 'text',
          tooltip: true
        },
        {
          title: '执行时间',
          key: 'date',
          sortType: 'desc'
        },
        {
          title: '申请人',
          key: 'username'
        },
        {
          title: '真实姓名',
          key: 'real_name'
        },
        {
          title: '执行人',
          key: 'assigned'
        },
        {
          title: '执行区域',
          key: 'computer_room'
        },
        {
          title: '连接名称',
          key: 'connection_name'
        },
        {
          title: '库名',
          key: 'basename'
        },
        {
          title: '操作',
          key: 'action',
          align: 'center',
          render: (h, params) => {
            return h('div', [
              h('Button', {
                props: {
                  size: 'small',
                  type: 'text'
                },
                on: {
                  click: () => {
                    this.$router.push({
                      name: 'orderlist',
                      query: { workid: params.row.work_id, id: params.row.id, status: 1, type: params.row.type }
                    })
                  }
                }
              }, '详细信息')
            ])
          }
        }
      ],
      TableDataNew: [],
      pagenumber: 1
    }
  },
  methods: {
    getrecordinfo (vl = 1) {
      axios.get(`${this.$config.url}/record?page=${vl}`)
        .then(res => {
          [this.TableDataNew, this.pagenumber] = [res.data.data, res.data.page]
        })
        .catch(error => {
          this.$config.err_notice(this, error)
        })
    },
    splicpage (page) {
      this.getrecordinfo(page)
    }
  },
  mounted () {
    this.getrecordinfo()
  }
}
</script>
