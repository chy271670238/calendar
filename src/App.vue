<template>
  <div>
    <Top />
    <AddCalendar  @getMessage="getMessage"/> 
    <List :calendarData="this.calendarData"  @getMessage="getMessage"/>
    <Bottom :complete="complete" :del="del"/>
  </div>
</template>

<script>
  import Top from './components/Top.vue';
  import AddCalendar from './components/AddCalendar.vue';
  import List from './components/List.vue';
  import Bottom from './components/Bottom.vue';
  export default {
    name:'App',
    components:{Top,Bottom,AddCalendar,List},
    data(){
      return{
        calendarData:[],
        complete:localStorage.complete,
        del:localStorage.del,
      }
      
    },
    methods: {
      // 为什么要把获取的日程独立成一个方法
      getMessage(){
        if(localStorage.info !=  undefined) this.calendarData=JSON.parse(localStorage.info);
        this.complete=localStorage.complete===undefined?0:localStorage.complete;
        this.del=localStorage.del===undefined?0:localStorage.del;
      }
    },
    mounted(){
      this.getMessage();
      // 阻止右键的默认行为
      document.addEventListener('contextmenu',event=>event.preventDefault());
    }
  };
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  list-style: none;
}
.app{
    width:100vw;
    height:100vh;
    background-color: rgba(222, 246, 226, 0.963);
    display:flex;
    flex-direction: column;
    // touch-action:none;
  }
</style>