<template>
    <div class="add">
        <input type="text" v-model="msg" >
        <button @click="saveCalendar">添加</button>
    </div>
</template>

<script>

import {nanoid} from 'nanoid';
    export default {
        name:'AddCalendar',
        data(){
            return{
                msg:''
            }
        },
        methods: {
            saveCalendar(){
                if(this.msg.trim()==''){
                    alert("不能输入空的日程")
                    return
                }
                let info={
                    id:nanoid(),
                    msg:this.msg
                }
                this.msg=''
                if(localStorage.info===undefined){
                    localStorage.info=JSON.stringify([info])
                }else{
                    let tempArr=JSON.parse(localStorage.info);
                    tempArr.push(info);
                    localStorage.info=JSON.stringify(tempArr);
                }
            }
        },
    }
</script>

<style lang="scss" scoped>
.add{
    width:100vw;
    display: flex;
    justify-content: space-around;
    input {
        flex:3;
        margin: 10px;
    }
    button {
        flex:1;
        margin: 10px;
    }
}
</style>