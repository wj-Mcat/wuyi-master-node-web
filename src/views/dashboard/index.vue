<template>
  <div class="dashboard-container">
    <el-row>
        <el-col :span="24">
          <el-card class="box-card">
            <div slot="header" style="width: 100%;" class="clearfix">
              <span>Hadop分布式系统</span>
            </div>
            <v-chart :options="option" style="width: 100%"  id='chart'/>
          </el-card>
          <el-card style="margin-top: 30px;">
            <div slot="header" style="width: 100%;" class="clearfix">
              <span>信息统计</span>
            </div>
            <table class="table b-table table-striped table-hover">
              <tbody>
                <tr>
                  <td>块大小</td>
                  <td>{{summary.dfs_blk_size}}KB</td>
                </tr>
                <tr>
                  <td>DataNode端口号</td>
                  <td>{{summary.data_node_port}}</td>
                </tr>
                <tr>
                  <td>NameNode端口号</td>
                  <td>{{summary.name_node_port}}</td>
                </tr>
                <tr>
                  <td>Host列表</td>
                  <td>{{summary.host_list}}</td>
                </tr>
                <tr>
                  <td>DFS冗余数</td>
                  <td>{{summary.dfs_replication}}</td>
                </tr>
              </tbody>
            </table>
          </el-card>
        </el-col>
    </el-row>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import ECharts from 'vue-echarts'
import 'echarts/lib/chart/line'
import 'echarts/lib/component/polar'

import { option } from './option'

import axios from 'axios'

export default {
  name: 'Dashboard',
  computed: {
    ...mapGetters([
      'name'
    ])
  }, 
  data() {
    return {
      option: option,
      summary: {},
    }
  },
  mounted() {
    let $this = this;
    axios.get('http://127.0.0.1:5000/all_status').then(function(data){
      data = data.data.data
      $this.option.legend.data = data.nodes
      $this.option.series = data.series
      $this.option.xAxis.data = data.time_lines
      console.log($this.option)
      $this.summary = data.summary
    })
    this.option.tooltip.formatter = function(data) {
      debugger
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
  .echarts, .echarts>div, .echarts > canvas{
    width: 100% !important;
  }
}
</style>
