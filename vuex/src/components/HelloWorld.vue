<template>
  <div class="hello">
    <head-box></head-box>
    <div class="main">
      <hospital-box v-for='(item, idx) in $store.state.hospital_list' :key='idx' :index='idx' :name="item.name" :hospital='item'>
      </hospital-box>
    </div>
    <foot-box></foot-box>
  </div>
</template>
<script>
  import hospitalBox from '@/components/hospitalBox'
  import footBox from '@/components/footBox'
  import headBox from '@/components/headBox'
  import Vuex from 'vuex'
  import Vue from 'vue'
  Vue.use(Vuex)
  const store = new Vuex.Store({
    state: {
      hospital_list: [],
      sum: 0
    },
    mutations: {
      inner (state, data) {
        state.hospital_list = data
      },
      change_hospital_checkall (state, data) {
        state.hospital_list.forEach((item, index) => {
          if (index == data.hospital_index) {
            item.check_all = data.checkall
            item.hospital_sub.forEach((value) => {
              value.checked = data.checkall
            })
          }
        })
      },
      medical_checked (state, data){
        //  每个单选是否全部选中
        state.hospital_list[data.hospital_index].hospital_sub[data.medical_index].checked = data.checkall
        // 定义一个全局 判断单选每个是否全部选中 
        var flag = true
        state.hospital_list.forEach((item, index) => {
          if (index == data.hospital_index) {
            item.hospital_sub.forEach((value, ind) => {
              if (!value.checked) {
                flag = false
              }
            })
          }
        })
        // 关于判读点击每个是否全选
        state.hospital_list.forEach((item, index) => {
          if (index == data.hospital_index) {
            item.check_all = flag
          }
        })
      },
      update_list (state, data) {
        console.log(data.hospital_index)
        state.hospital_list[data.hospital_index].hospital_sub[data.medical_index].count = data.count

      },
      update_sum (state) {
        let sumNumber = 0
        state.hospital_list.forEach((item, index) => {
            item.hospital_sub.forEach((value, ind) => {
              if (value.checked) {
                sumNumber += value.price * value.count
              }
            })
        })
        state.sum = sumNumber;
        console.log(state.sum)
      }
    },
    actions: {
      fetch_hospital_list ({commit}) {
        console.log(commit)
        fetch('/src/api/data.json')
        .then((Response) => {
          return Response.json()
        }).then((res) => {
          commit('inner', res.hospital_list)
        })
      }
    }
  })
  store.subscribe(function (data) {
    // console.log(data)
  })
  export default {
    store: store,
    data () {
      return {
        hospitalData: [],
        sum: 0
      }
    },
    components: {
      hospitalBox,
      footBox,
      headBox
    },
    created () {
      this.$store.dispatch('fetch_hospital_list')
    }
  }
</script>
<style scoped>
  .hello {
    width: 100%;
    height: 100%;
    display: -webkit-flex;
    flex-direction: column;
    
  }
  .main {
    flex: 1;
    overflow-y: scroll;
  }
</style>
