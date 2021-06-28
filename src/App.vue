<template>
  <div id="app">
    <el-dialog title="提示" :visible.sync="dialogVisible" width="30%" >
      <!-- :before-close="handleClose" -->
      <span>让我们开始吧   </span> 
      <span  class="dialog-footer">
        <!-- slot="footer" -->
        <el-button type="primary" @click="beginload">确 定</el-button>
      </span>
    </el-dialog>
    <el-dialog title="复制结果" :visible.sync="enddialogVisible" width="30%" >
      <!-- :before-close="handleClose" -->
      <span>点击复制结果到剪切板   </span> 
      <span  class="dialog-footer">
        <!-- slot="footer" -->
        <el-button type="primary" v-clipboard:copy="resultdataString">复 制</el-button>
      </span>
    </el-dialog>
    <div v-for="(item,i) in list" :key="i" v-bind:class="{default:item.isactive1,default1:item.isactive2}" @click="clickdiv(item)" v-bind:style="{width:divwidth+ 'px',height:divheight+ 'px'}">{{item.id}}</div>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import ElementUI from 'element-ui';  
import Vue from 'vue';
import Clipboard2 from 'vue-clipboard2';
import 'element-ui/lib/theme-default/index.css';

Vue.use(ElementUI);
Vue.use(Clipboard2);

var vm = new Vue();

export default {
  name: 'App',
  data(){
    return{
      list:[{id:1,isactive:true},{id:2,isactive:true}],
      isactive:true,
      dialogVisible:true,
      enddialogVisible:false,
      begintime:0,
      endtime:0,
      divwidth:40,
      divheight:50,
      resultdata:[],
      times:0,
      resultdataString:"",
    }
  },
  components: {
    // HelloWorld
  },
  mounted(){   //页面初始化方法
    console.log("初始化")
    this.divwidth=parseInt(window.innerWidth/6)-2;
    console.log(this.divwidth)
    this.divheight=parseInt(window.innerHeight/9)-2;
    console.log(this.divheight)

	},
  methods:{
    beginload() {
      console.log("调用到了---");
      console.log(window.innerWidth);
      console.log(window.innerHeight);
      console.log(parseInt(window.innerWidth/this.divwidth));
      console.log(parseInt(window.innerHeight/this.divheight));      
      this.dialogVisible=false;
      var length =parseInt(window.innerWidth/this.divwidth)*parseInt(window.innerHeight/this.divheight);
      console.log(length);
      var randomint = parseInt(Math.random() * length + 1)
      for(var i=0;i<length;i++){
        if(randomint === i+1){
          vm.$set(this.list,i,{id:i+1,isactive2:true,shouldclick:true})
        }else{
          vm.$set(this.list,i,{id:i+1,isactive1:true,shouldclick:false})

        }
        // console.log(this.list[i]);
        
      }
      this.begintime=new Date().getTime();
      console.log("begintime:"+this.begintime);

    },
    clickdiv(item){
      if(this.times<10){
        this.times=this.times+1;
        this.endtime=new Date().getTime();
        var timestamp=this.endtime-this.begintime;
        this.resultdata[this.times]={time:timestamp,clickright:item.shouldclick,index:item.id};
        console.log(this.resultdata[this.times]);
        console.log("endtime:"+this.endtime);
        this.beginload();
      }else{
        console.log(this.resultdata);
        this.resultdataString=JSON.stringify(this.resultdata);
        this.enddialogVisible=true;
      }

    }

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.default {
  box-sizing: border-box;
  background-color:#F00;
  /* width: 97px;
  height: 120px; */
  display: inline-flex ;
  border: 1px solid  #2c3e50;
  justify-content: center;
  align-items: center;
}

.default1 {
  box-sizing: border-box;
  background-color:rgb(0, 255, 157);
  /* width: 97px;
  height: 120px; */
  display: inline-flex ;
  border: 1px solid  #2c3e50;
  justify-content: center;
  align-items: center;
}

</style>
