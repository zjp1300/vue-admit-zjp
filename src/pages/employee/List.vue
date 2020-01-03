<template>
    <div>
        <!-- 按钮 -->
        <div>
            <el-button size="smll" type="primary" @click="toAddHandler">添加</el-button>
            <el-button size="smll" type="danger">批量删除</el-button>
        </div>
        <!-- /按钮 -->
        <!-- 表格 -->
        <el-table :data="employees">
            <el-table-column label="编号" prop="id"></el-table-column>
            <el-table-column label="用户名" prop="username"></el-table-column>
            <el-table-column label="姓名" prop="realname"></el-table-column>
            <el-table-column label="职位"  prop="type"  > </el-table-column>
            <el-table-column label="身份证号" prop="idcard"></el-table-column>
            <el-table-column label="手机号" prop="telephone"></el-table-column>
            <el-table-column label="银行卡号" prop="bankid"></el-table-column>
            <el-table-column label="操作" fixed="right"> 
                <template v-slot="slot">
                    <a href="" @click.prevent="todeleHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUppdateHandler(slot.row)">修改</a>
                </template>
            </el-table-column>
        </el-table>
        <!-- 分页 -->
        <el-pagination
    layout="prev, pager, next"
    :total="50">
  </el-pagination>
  <!-- 模态框 -->
  <el-dialog
  :title="title"
  :visible.sync="visible"
  width="60%">
  测试：{{form}}
 <el-form label-width="90px" :model="form">
     <el-form-item label="用户名">
<el-input v-model="form.username">  </el-input>
     </el-form-item> 
      <el-form-item label="密码">
<el-input type="password" v-model="form.password">  </el-input>
     </el-form-item> 
      <el-form-item label="姓名" >
<el-input v-model="form.realname">  </el-input>
     </el-form-item> 
       <el-form-item label="密码">
<el-input v-model="form.password">  </el-input>
     </el-form-item> 
 <el-form-item label="性别">
     <el-radio-group >
    <el-radio label="男">男</el-radio>
    <el-radio label="女">女</el-radio>
  </el-radio-group>
 </el-form-item> 
      <el-form-item label="手机号">
<el-input v-model="form.telephone" >  </el-input>
     </el-form-item> 
      <el-form-item label="身份证号">
<el-input v-model="form.idCard">  </el-input>
     </el-form-item>
      <el-form-item label="银行卡号">
<el-input v-model="form.bankCard">  </el-input>
     </el-form-item>
 </el-form>
  <span slot="footer" class="dialog-footer">
    <el-button size="small" @click="closeModalHandler">取 消</el-button>
    <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
  </span>
</el-dialog>
        <!-- /按钮 -->


    </div>
</template>
<script>
import request from'@/utils/request'
import querystring from'querystring'
export default {
    data(){
        return{
            title:"录入员工信息",
            visible:false,
            employees:[{ } ], 
            form:{
                type:"waiter"
            } 
        }
    },
    created(){
        this.loadData();
    },
    
    methods:{
        submitHandler(){
           let url="http://localhost:6677/waiter/saveOrUpdate";
           //前端向后台发送数据，完成数据的保存
            request({
                url,
                method:"post",
            //告诉后台我的请求体中放的是查询字符串
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
            //请求体中的数据，将this.from转换为查询字符串发送给后台
                data:querystring.stringify(this.form)
            }).then((response)=>{
                //请求结束 模态框关闭
                this.closeModalHandler();
                //刷新
               this.lodaData();
                //提示消息
                this.$message({
                    type:"success",
                    message:response.message
                })
            })
        },
        //加载员工数据
        loadData(){
        let url="http://localhost:6677/waiter/findAll"
        request.get(url).then((response)=>{
            //箭头中的this指向外部的函数中方的this
            this.employees=response.data;
        })

        },
        closeModalHandler(){
            this.visible=false;
        },
        toAddHandler(){
            this.title="录入员工信息";
             this.visible=true;
        },
        todeleHandler(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        })

        },
        toUppdateHandler(){ 
            this.title="修改员工信息";
              this.visible=true;
        },
         created(){
//vue实例加载完毕
this.lodaData();
    }
    }
}
</script>
<style scoped>

</style>