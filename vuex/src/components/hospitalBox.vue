<template>
    <div class="hospital-box" ref='boxshow' v-if='boxnon'>
        <p @click='checkeds'>
            <span class="span-box" :class="{'span-box-active': hospital.check_all}"></span>{{name}}
        </p>
         <hospital-list v-for='(val, idx) in hospital["hospital_sub"]' :ids='idx' :key='idx' :idn='index' :checkbtns='check' :hospitals='val'></hospital-list> 
    </div>
</template>
<script>
    import hospitalList from '@/components/hospitalList'
    export default {
      props: {
        name: {
          type: String,
          required: true
        },
        hospital: {
          type: Object,
          required: true
        },
        index: {
          type: Number,
          required: true
        }
      },
      data () {
        return {
          boxnon: true,
          check: false
        }
      },
      components: {
        hospitalList
      },
      methods: {
        checkeds () {
          this.$store.commit('change_hospital_checkall', {
            hospital_index: this.index,
            checkall: !this.hospital.check_all
          })
          this.$store.commit('update_sum')
        }
      }
    }
</script>
<style scoped>
    .hospital-box {
        width: 100%;
        line-height: 1rem;
        box-shadow: 0 0 2px #ccc;
        margin-bottom: 10px;
        overflow: hidden;
    }
    .hospital-box p {
        border-bottom: 1px solid #ccc;
    }
    .span-box {
      display: inline-block;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      margin: 0 10px;
      border: 1px solid #ccc;
      position: relative;
    } 
    .span-box-active::after {
      content: " ";
      width: 10px;
      height: 10px;
      background: #333;
      border-radius: 50%;
      position: absolute;
      left: 0;
      top: 0;
      margin-left: 5px;
      margin-top: 5px;
    }
</style>