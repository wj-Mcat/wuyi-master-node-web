<template>
  <div class="dashboard-container">
    <v-chart :options="option" />
    <br/>
    <br/>
    <br/>
    <br/>
    代码参考地址：https://echarts.apache.org/examples/en/editor.html?c=line-stack
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
      option: option
    }
  },
  mounted() {
    let $this = this;
    axios.get('http://127.0.0.1:5000/all_status').then(function(data){
      data = data.data.data
      debugger

      $this.option.legend.data = data.nodes
      $this.option.series = data.series
      $this.option.xAxis.data = data.time_lines
      console.log($this.option)
    })
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
}
</style>
