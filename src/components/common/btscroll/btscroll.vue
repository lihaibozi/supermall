<template>
    <div class="wrapper" ref="wrapper">
      <div class="content">
          <slot></slot>
      </div>
    </div>
</template>

<script>
    import btscroll from 'better-scroll'
    export default {
        name: "btscroll",
        props:{
          probeType:{
            type:Number,
            default:0
          },
          pullUpLoad:{
            type:Boolean,
            default:false
          }
        },
        data(){
          return{
            scroll:null
          }
        },
      mounted(){
         //创建滚动对象
          this.scroll = new btscroll(this.$refs.wrapper,{
            click:true,
            probeType:this.probeType,
            pullUpLoad:this.pullUpLoad
          })
        //监听滚动位置
         this.scroll.on('scroll',(position)=>{
           this.$emit('homeScroll',position)
         })
        //监听上拉事件
        this.scroll.on('pullingUp',()=>{
          this.$emit('pullingUp');

        })
      },
      methods:{
        scrollTo(x,y,time=1000){
          this.scroll.scrollTo(x,y,time);
        },
        finishPullingUp(){
          this.scroll.finishPullUp();
        }
      }
    }
</script>

<style scoped>
</style>
