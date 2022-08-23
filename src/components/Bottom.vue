<template>
    <div>
        <div class="bottom">
            完成：{{complete}}&nbsp;&nbsp;&nbsp;&nbsp;失败：{{del}}
        </div>
    </div>
</template>

<script>
    export default {
        name:'Bottom',
        props:['complete','del']
    }
</script>

<style scoped>
.bottom{
    width:100vw;
    height:60px;
    background-color:skyblue ;
    color:white;
    font-size: 20px;
    text-align: center;
    line-height: 60px;  
    position: fixed;
    bottom: 0; 
}
</style>