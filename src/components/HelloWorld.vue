<template>
  <div class="hello">
    <div class="box">
      <h1>请输入你的信息</h1>
      <el-input v-model="userName" placeholder="请输入你的名字"></el-input>
      <el-input v-model="userAge" placeholder="请输入你的年龄"></el-input>
      <el-input v-model="userAddress" placeholder="请输入你的地址"></el-input>
      <el-button type="primary" @click="submit">点击提交</el-button>
    </div>
    <div class="pox">
      <h1>获取你的信息</h1>
      <el-input v-model="infoAge" placeholder="请输入age条件"></el-input>
      <el-button type="primary" @click="getInfo">获取你输入的信息</el-button>
    </div>
    <div class="info"  v-if="tableData.length>0">
            <el-table
              :data="tableData"
              style="width: 100%">
              <el-table-column
                prop="name"
                label="姓名"
                width="180">
              </el-table-column>
              <el-table-column
                prop="age"
                label="年龄"
                width="180">
              </el-table-column>
              <el-table-column
                prop="address"
                label="地址">
              </el-table-column>
            </el-table>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
const domain = 'http://localhost:3000'
export default {
  name: 'HelloWorld',
  data () {
    return {
      userName: '',
      userAge:'',
      userAddress:'',
      infoAge:'',
      tableData:[]
    }
  },
  mounted:function(){

  },
  methods:{
    submit:async function(){
      let userInfo = {
        name:encodeURIComponent(this.userName),
        age:this.userAge,
        address:encodeURIComponent(this.userAddress)
      }
      let {status,data} = await axios.post(domain+'/addInfo',userInfo);
      if(data.code == 0){
          this.$message({
            showClose: true,
            message: '提交成功',
            type: 'success'
          });
      }else{
        this.$message({
          showClose: true,
          message: '保存失败',
          type: 'warning'
        });
      }
      

    },
    getInfo:async function(){
      let filterData = {
        age:this.infoAge
      }
      let {status,data} = await axios.post(domain+'/getUserInfo',filterData);
      data.result.map((item,index)=>{
        item.name =decodeURIComponent(item.name);
        item.address =decodeURIComponent(item.address);
      })
      this.tableData = data.result
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
h1, h2 {
  font-weight: normal;
}
h2{
  color: red;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.el-input__inner{
  width:200px;
  height: 40px;
  border:1px solid green;
  margin-bottom: 10px;
}
.info{
  width:500px;
  margin:0 auto;
}
</style>
