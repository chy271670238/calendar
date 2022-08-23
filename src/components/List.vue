<template>
    <div class="List">
        <h1>待办列表：</h1>
        <transition-group tag="ul" ref="calendar">
            <li v-for="item of calendarData" :key="item.id" :data-index="item.id">
                <p>{{item.msg}}</p>
                <div class="control">
                    <button @click="del($event)">删除</button>
                    <button @click="complete($event)">完成</button>
                </div>
            </li>
        </transition-group>
    </div>
</template>

<script>
    export default {
        name:"List",
        props:['calendarData'],
        methods:{
            //给元素挂载一个长按事件
            longPress(ele,func,timeout=500){
                let timer;
                ele.addEventListener("touchstart",event=>{ //触摸开始，将之前的定时清除
                    clearTimeout(timer);
                    timer=setTimeout(() => {
                        func(event); 
                    }, timeout);
                });
                ele.addEventListener("touchmove",(event)=>{   //触摸移动了
                    clearTimeout(timer);
                });
                ele.addEventListener("touchend",(event)=>{    //触摸结束
                    clearTimeout(timer);
                })
                ele.addEventListener("touchcancel",(event)=>{ //触摸停止
                    clearTimeout(timer);
                })

            },

            del(event){
                //获取事件的触发者p 的父节点（control）的父节点（li）下的item.id
                let index=event.target.parentNode.parentNode.dataset.index
                let temp=this.calendarData.filter((item)=>item.id!=index); //过滤掉当前id后的内容，即删除
                localStorage.info=JSON.stringify(temp);
                localStorage.del===undefined?localStorage.del=1:localStorage.del++
                this.$emit("getMessage")
            },
            complete(event){
                let index=event.target.parentNode.parentNode.dataset.index
                let temp=this.calendarData.filter((item)=>item.id!=index); 
                localStorage.info=JSON.stringify(temp);
                localStorage.complete===undefined?localStorage.complete=1:localStorage.complete++
                this.$emit("getMessage")
            },

        },
        mounted(){
            //将长按事件绑定到ul元素上形成一个事件委托
            this.longPress(this.$refs.calendar.$el, (event)=>{
                document.querySelectorAll('.control').forEach(item=>item.style.display="none");
                //排它锁，如果所有的control，有一个展开了，其他的control全部隐藏
                //console.log(event.target);  //p
                event.target.nextElementSibling.style.display="block";
                //p 标签的 下一个元素的样式
            })
        }
    }
</script>

<style lang="scss" scoped>
.v-enter,.v-leave-to{
    opacity: 0;
}
.v-enter-to,.v-leave{
    opacity: 1;
}
.v-enter-active{
    transform: all 500ms linear;
}
.v-leave-active{
    transition: all 500ms linear;
    position: absolute; //脱流
}
.v-move{
    transition: all 500ms linear;
}
.List{
    flex:1;
    overflow: hidden;
    padding:10px;
    display:flex;
    flex-direction: column;
    h1{
        font-size:18px;
    }
    ul{
        flex:1;
        overflow: scroll;
        li{
            width:100%;
            box-sizing: border-box;
            padding:0 10px;
            margin:5px 0;
            line-height: 35px;
            &:nth-child(2n){
                background-color: rgb(254, 237, 237);
            }
            &:nth-child(2n-1){
                background-color: rgb(224, 224, 248);
            }
            button{
                border: 0;
                background-color: white;
                padding:6px;
                margin-left: 10px;
                
            }
            .control{
                text-align:right;
                display: none;
            }
        }
    }
}
</style>