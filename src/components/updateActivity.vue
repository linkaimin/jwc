<template>
  <div>
    <el-container class="con">
      <el-header class="header" height="80px">
        <el-row>
          <el-col :span="12"><div class="grid-content bg-purple"><img src="../assets/headLogo.png" class="header-logo"/> </div></el-col>
          <el-col :span="12"><div class="grid-content bg-purple-light text-right">
           <span ><i class="el-icon-share i_rd"></i><span @click="exit">安全退出</span></span>
          </div></el-col>
        </el-row>
      </el-header>
      <el-container>
        <el-aside width="220px" class="aside">
     <div class="mean-top"><i class="el-icon-menu"></i> 功能导航</div>
          <el-menu  router :default-active="$route.path"
                    background-color="#dbe9f1"
                    active-text-color="#6ec673" id="menu"
                    >
            <el-submenu index="4">
              <template slot="title"><i class="el-icon-location"></i>评估结果统计</template>
              <el-menu-item-group >
                <el-menu-item index="/show"><i class="el-icon-tickets"></i>评估结果展示</el-menu-item>
               
              
              </el-menu-item-group>
            </el-submenu>
           
            <el-submenu index="8">
              <template slot="title"><i class="el-icon-location"></i>项目管理</template>
              <el-menu-item-group>
                <el-menu-item index="/newActivity"> <i class="el-icon-tickets"></i>新增活动</el-menu-item>
                <el-menu-item index="/manage"> <i class="el-icon-tickets"></i>活动管理</el-menu-item>
                <el-menu-item index="/addActivity"> <i class="el-icon-tickets"></i>活动项目添加</el-menu-item>
                <el-menu-item index="/manageActivity"> <i class="el-icon-tickets"></i>活动项目管理</el-menu-item>
               

              </el-menu-item-group>
            </el-submenu>
            <el-submenu index="9">
              <template slot="title"><i class="el-icon-location"></i>用户管理</template>
              <el-menu-item-group>
                <el-menu-item index="/newUser"><i class="el-icon-tickets"></i>新增用户</el-menu-item>
                <el-menu-item index="/newAdmin"><i class="el-icon-tickets"></i>新增管理员</el-menu-item>
                <el-menu-item index="/manageUser"><i class="el-icon-tickets"></i>用户信息管理</el-menu-item>
              

              </el-menu-item-group>
            </el-submenu>

            </el-menu>
        </el-aside>
                   <el-card id="card" class="box-card">
              <div slot="header" class="clearfix">
                <span>修改活动</span> 
              </div>
              <div class="item">
              活动名称：<el-input class="activity" v-model="name" placeholder="请输入内容"></el-input>
               </div><div class="item" >
              
               所属单位：<el-input class="activity" v-model="unit" placeholder="请输入内容"></el-input>
              </div><div class="item" >
               <div class="item">
              相关信息：<el-input class="activity" v-model="info" placeholder="请输入内容"></el-input>
               </div>
             <div class="block">
                 开始时间：
    <el-date-picker 
      v-model="value1"
      type="datetime"
      placeholder="选择日期时间">
    </el-date-picker>
  </div>
            </div><div class="item" >
             <div class="block">
                 结束时间：
    <el-date-picker 
      v-model="value2"
      type="datetime"
      placeholder="选择日期时间">
    </el-date-picker>
  </div>
              </div>
                 <el-button  type="text" @click="dialogFormVisible = true">选择活动相关用户</el-button>

<el-dialog title="用户姓名" :visible.sync="dialogFormVisible">
  <el-form :model="form1">
     <el-checkbox-group v-model="list" >
        <label id="checkbox" v-for="item in user" :key = item.userId>
    <el-checkbox :label=item.userId >{{item.userName}}|单位：{{item.unit}}</el-checkbox>
  </label>

  </el-checkbox-group>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
  </div>
</el-dialog>
<br>
     <el-button type="text" @click="dialogFormVisible1 = true">选择分数统计方法</el-button>

<el-dialog title="统计方法" :visible.sync="dialogFormVisible1">
  <el-form :model="form2">
    <el-checkbox-group 
    v-model="tag"
    :max="1">
    <el-checkbox :label="1" >所有专家总分取平均分</el-checkbox>
    <br>
     <el-checkbox :label="2" >所有专家每一项打分先取平均分，然后取和</el-checkbox>
      <br>
      <el-checkbox :label="3" >所有专家每一项打分去掉一对最高分和最低分后每一项平均分，然后取和</el-checkbox>
      <br>
       <el-checkbox :label="4" >所有专家总分去掉一对最高分和对低分后取平均分</el-checkbox>
  </el-checkbox-group>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible1 = false">取 消</el-button>
    <el-button type="primary" @click="dialogFormVisible1 = false">确 定</el-button>
  </div>
</el-dialog>
 <el-main>
    <el-col :span="24" class="warp-main" v-loading="">
      <el-form :inline="true" class="demo-form-inline" v-for="(item, i) in FormArr" :key="i">
        <el-form-item label="评分方向：">
          <el-input v-model="item.lname" placeholder="例：创新性"></el-input>
        </el-form-item>
         <el-form-item label="总分占比：">
          <el-input v-model="item.part" placeholder="例：0.30"></el-input>
        </el-form-item>
        <el-button type="primary" @click="Delete(item.index)">删除</el-button>
      </el-form>
      <el-button type="primary" @click="AddForm">增加更多</el-button>
      
    </el-col>
  </el-main>
              <div id='btn'>
               <el-button id="button" @click="add"  type="primary" plain>确定</el-button>
               </div>
            </el-card>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  data(){
    return{
      FormArr: [
        {
          lname: '',
          part:'',
          index:'0'
        }
      ],
     part:1,
      list:[],
      tag:[],
      userName:'',
      userId:'',
      name:"",
      info:"",
      unit:"",
      value1: '',
      value2: '',
        dialogFormVisible1: false,
        dialogFormVisible: false,
        form1: {
          name: '',
          region: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
         form2: {
          name: '',
          region: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
        formLabelWidth: '100px',
        user:[],
    }
  },
  mounted(){
  this.text()
  this.lis()
  },
  methods: {
      AddForm :function() {
      this.FormArr.push({
          lname: '',
          part:'',
          index: this.FormArr.length,
      })
      this.part = (1/this.FormArr.length).toFixed(2)
      console.log(this.FormArr)
    },
    Delete:function (index) {
      this.FormArr.splice(index, 1)
     this.part = (1/this.FormArr.length).toFixed(2)
    },
     lis(){
var that = this;
     this.$axios({
      data:{
	    "role" : "2"
      },
      method:"post",
      url:'/user'
     })
     .then(function (response){
       console.log(response);
      if (response.data.resultCode === 200) {
        that.user = response.data.data
       console.log(that.user)
      }
     })
     },
      text(){
        var data = this.$route.query.ruleForm;
        console.log(data)
        this.name = data.name;
        this.unit = data.unit;
        this.info = data.info;
        this.value1 = data.startTime;
        this.value2 = data.endTime;
        this.activityId = data.activityId;
        if(data.list !== ''){
          for(let i of data.list)
        this.list.push(i.userId); 
        }
        if(data.list !== ''){
        this.FormArr = data.tag;  
        }
        this.tag.push(data.type);
        console.log(this.list)
      },
      
 exit: function () {
      var that = this;
             this.$axios.get('/logout', {

  })
  .then(function (response) {
    console.log(response);
      if (response.data.resultCode === 200) {
        sessionStorage.clear()
        that.$message({
            message: '退出成功',
            type: 'success',
            duration: 2000
          })
          that.$router.push('/')
       
        } else {
          that.$router.push('/')
        }
  })
  .catch(function (error) {
    console.log(error);
  });
    } ,
   add: function () {
      var that = this;
       var sum = 0;
    for(let i of that.FormArr){
     sum += Number(i.part);
    }
    if(sum != 1){ 
       this.$message({
            message: '评分占比和不为1！请重新填写！',
            type: 'error',
            duration: 2000
          })
          return;
    }
      console.log("that.value1 = " + that.value1);
 console.log("that.value2 = " + that.value2);
if(that.name!=''&& that.value1!='' && that.value1!=null &&that.value2!='' && that.value2 != null &&that.unit!=''&&!(JSON.stringify(that.list) === '[]')&&that.tag.length != 0&&that.FormArr[0].part != ''&&that.FormArr[0].lname!= undefined){

             this.$axios({
      data:{
        "name" : that.name,
        "info" : that.info,
        "startTime" : that.value1,
        "endTime" : that.value2,
        "unit" : that.unit,
        "list":that.list,
        "type":that.tag.pop(),
        "tag" : that.FormArr,
        "activityId":that.activityId
      },
      
      method:"put",
      url:'/activity'
  })
  .then(function (response) {
    console.log(response);
      if (response.data.resultCode === 200) {
        sessionStorage.clear()
        that.$message({
            message: '修改成功',
            type: 'success',
            duration: 2000
          })
      that.$router.push('/manage')
      
        } else {
          that.$message({
            message: '失败，可能是网络故障',
            type: 'error',
            duration: 2000
          })
        }
  })
  .catch(function (error) {
    console.log(error);
  });
  }else{
       that.$message({
            message: '有还未填的信息！无法提交！',
            type: 'error',
            duration: 2000
          })
    } 
    }  
    },
    handleClose(tag) {
      this.dynamicTags.splice(this.dynamicTags.indexOf(tag), 1);
    },
   
  }

</script>

<style>
.activity{
  width:80%;
}
#btn{
  margin:5% auto;
  width:68px;
}
.item{

  text-align:center; 
}
.activity .unit .info{
  width: 70%;
}
  #card{
    margin:3% auto;
  }
  .text {
    font-size: 14px;
  }

  .item {
    margin-bottom: 18px;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 480px;
  }
a{
  text-decoration:none; 
  color:rgb(21, 46, 112);
}
.con{
  width: 100%;
  height: 100%;
  position: absolute;
}
*{
  margin: 0; padding: 0;
}
.test{
  width: 500px;
  height: 500px;
  background-color: red;
}
.header{
  width: 100%;


  background-repeat: no-repeat;
  background-size: 100% 100%;
  color: #ffffff;
  background-color:#0d3349 ;
}
  .header-logo{
    margin-top: 8px;
    height: 64px;
  }
  .text-right{
    text-align: right;
    line-height: 80px;
  }
  .text-right span{
    margin-right: 10px;
  }
  .text-right span i{
    margin-right: 5px;
  }

  .aside{
    background-color: rgba(219, 233, 241, 0.45);
  }
  .el-submenu{
    border-bottom: 1px solid #bbe0f6;

  }
  .mean-top{
    width: 100%;
    height: 41px;
    line-height: 41px;
    text-indent: 5px;
  }
  .main{
    background-color: #eeeeee;
    width: 100%;
    padding: 0px;
  }
  .main-tab{
    width: 100%;
    line-height: 40px;
    border-bottom: 2px solid #0d3349;
    height: 40px;
  }

</style>
