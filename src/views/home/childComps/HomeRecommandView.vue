<template>
  <div class="recommend">
      <div :key="index" v-for="(item,index) in recommends" class="recommendItem">
         <a :href="item.link">
           <img :src="item.image" alt="">
           <div>{{item.title}}</div>
         </a>
      </div>
  </div>
</template>

<script>
    export default {
        name: "HomeRecommandView",
        props:{
          recommends:{
            type:Array,
            default(){
              return []
            }
          }
        }
    }
</script>

<style scoped>
    .recommend{
      display: flex;
      width: 100%;
      text-align: center;
      font-size: 12px;
      padding: 10px 0 20px;
      border-bottom: 8px solid #eee;
    }
  .recommendItem{
    flex: 1;
  }
  .recommendItem img{
    width: 70px;
    height: 70px;
    margin-bottom: 5px;
  }
</style>
