<template>
    <div class="foot-box">
        <p>
            <span><b>总金额</b></span>
            <span>{{sum}}</span>
        </p>
        <p>
            <span>申请分期</span>
        </p>
    </div>
</template>
<script>
    export default {
      props: {
        sums: {
            type: Number
        }
      },
      computed: {
        sum () {
            return this.$store.state.sum
        }
      }
    }
</script>
<style scoped>
    .foot-box {
        width: 100%;
        height: 1rem;
        line-height: 1rem;
        box-shadow: 0 0 2px #ccc;
        display: -webkit-flex;
    }
    .foot-box>p:first-child {
        flex: 1;
    }
    .foot-box>p:last-child {
        width: 25%;
        color: #fff;
        text-align: center;
        background: orangered;
    }
</style>