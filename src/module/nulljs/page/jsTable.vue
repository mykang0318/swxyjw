<template>
    <div>
      <div></div>
      <div>
        <table>
          <tr>
            <th>校区</th>
            <th>教学楼</th>
            <th>教室名称</th>
            <th>座位数量</th>
          </tr>
          <tr v-for="(value, index) in mydata" :key="index">
            <td>{{value.xqmc}}</td>
            <td>{{value.jzwmc}}</td>
            <td>{{value.jsmc}}</td>
            <td>{{value.zws}}</td>
          </tr>
        </table>
      </div>
    </div>
</template>

<script>
import * as qz from '../../../api/qz'
import { mapActions } from 'vuex'
export default {
  name: 'jsTable',
  data () {
    return {
      mydata: []
    }
  },
  methods: {
    ...mapActions(['updataQzInfo'])
  },
  created () {
    const token = this.$store.state.qz.token
    if (token == null || token === '-1' || String(token).length !== 128) {
      console.log('updataqzinfo更新了数据')
      this.updataQzInfo()
    }
    const paeams = this.$route.params
    qz.getQz(paeams, token).then(res => {
      for (let i = 0; i < res.data[0].jsList.length; i++) {
        this.$set(this.mydata, i, res.data[0].jsList[i])
      }
    })
  }
}
</script>

<style scoped>
  table{
    width: 100%;
    text-align: center;
    border-collapse:collapse;
  }
  td, th{
    height: 2rem;
    border: #7d837e solid 1px;
    width: 25%
  }
  tr:nth-child(even) {
    background: #fffbff;
  }
  tr:nth-child(odd) {
    background: rgba(183, 191, 189, 0.27);
  }

</style>
