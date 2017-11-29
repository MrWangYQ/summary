<template>
    <div class="hospital-list" v-if='non' v-swipe:left="cb_left" v-swipe:right="cb_right">
        <div class="left">
           <span class="span-box" @click='medical_checked' :class="{'span-box-active': hospitals.checked}"></span>{{hospitals.sub_list}}
            <div class="left-p">
                <p><b>金额：￥</b><span ref='evePrice'>{{hospitals.price}}</span></p>
                <p>
                    <span class="raduce" @click='reduce'>-</span>
                    <input type="text" class="numBtn" :value='hospitals.count'>
                    <span class="add" @click='add'>+</span>
                </p>
            </div>
        </div>
        <div class="right" :class="{right_active: del_btn}" @click='delBtn'>
            <div>删<br/>除<br/></div>
        </div>
    </div>
</template>
<script>
  export default {
    props: {
      hospitals: {
        type: Object,
        required: true
      },
      ids: {
          type: Number,
          require: true
      },
      idn: {
          type: Number,
          require: true
      }
    },
    data () {
      return {
        value: 1,
        del_btn: false,
        data: [],
        non: true,
        checkbtn: false
      }
    },
    methods: {
      reduce () {
        this.$store.commit('update_list', {
            hospital_index: this.idn,
            medical_index: this.ids,
            count: --this.hospitals.count
        })
        this.$store.commit('update_sum')
      },
      add () {
        this.$store.commit('update_list', {
            hospital_index: this.idn,
            medical_index: this.ids,
            count: ++this.hospitals.count
        })
        this.$store.commit('update_sum')
      },
      cb_left () {
        this.del_btn = true
      },
      cb_right () {
        this.del_btn = false
      },
      delBtn () {
        this.non = !this.non
      },
      select () {
        this.checkbtn = true
      },
      medical_checked () {
        this.$store.commit('medical_checked', {
          hospital_index: this.idn,
          medical_index: this.ids,
          checkall: !this.hospitals.checked
        })
        this.$store.commit('update_sum')
      }
    }
  }
</script>
<style scoped>
    .hospital-list {
        display: block;
        line-height: .8rem;
        border-bottom: 1px solid #ccc;
        position: relative;
    }
    .left-p {
        display: -webkit-flex;
        width: 94%;
        padding-left: 7%;
    }
    .left-p p:last-child{
        text-align: right;
        margin-right: .5rem;
    }
    .hospital-box p:first-child label input {
        margin-right: 1rem;
    }
    .left label input{
        margin-right: 10px;
    }
    .left-p p{
        flex: 1;
    }
    .left-p p:last-child span {
        display: inline-block;
        width: .5rem;
        height: .4rem;
        line-height: .4rem;
        text-align: center;
        border: 1px solid #ccc;
        font-size: .3rem;
    }
    .numBtn {
        width: .5rem;
    }
    .right {
        position: absolute;
        right: 0;
        top: 0;
        bottom: 0;
        width: 0.8rem;
        text-align: center;
        color: #fff;
        background: red;
        transform: translateX(100%);
        transition: translateX .3s linear;
    }
    .right_active {
        transform: translateX(0);
    }
    .span-box {
      display: inline-block;
      width: 15px;
      height: 15px;
      border-radius: 50%;
      margin: 0 10px;
      vertical-align: middle;
      border: 1px solid #ccc;
      position: relative;
    } 
    .span-box-active::after {
      content: " ";
      width: 9px;
      height: 9px;
      background: #333;
      border-radius: 50%;
      position: absolute;
      left: 0;
      top: 0;
      margin-left: 3px;
      margin-top: 3.4px;
    }
</style>