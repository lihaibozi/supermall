<template>
  <div class="tablControl">
    <div v-for="(item,index) in titles" :key="item.index" class="tabControlItem"
         :class="{active:index===currentIndex}" @click="itemClick(index)">
      <span>{{item}}</span>
    </div>
  </div>
</template>

<script>
    export default {
        name: "TabBarControl",
        props:{
          titles:{
            type:Array,
            default(){
              return []
            }
          }
        },
      data(){
          return{
            currentIndex:0
          }
      },
      methods:{
        itemClick(index){
          this.currentIndex = index;
          this.$emit('tabControl',index);
        }
      }
    }
</script>

<style scoped>
  .tablControl{
    display: flex;
    text-align: center;
    font-size: 15px;
    height: 40px;
    line-height: 40px;
  }
  .tabControlItem{
    flex: 1;

  }
  .active{
    color:var(--color-high-text);
  }
  .tabControlItem span{
    padding: 5px;
  }
  .active span{
    border-bottom: 3px solid var(--color-tint);
  }
</style>
