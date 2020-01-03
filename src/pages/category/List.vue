<template>
<!-- 项目管理 -->
<div>
  <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
  <el-button type="danger" size="small">批量删除</el-button>
  <el-table :data="categorys">
<el-table-column prop="id" label="编号">  </el-table-column>
<el-table-column prop="name" label="栏目名称">  </el-table-column>
<el-table-column prop="num" label="序号"> </el-table-column>
<el-table-column prop="parentId" label="父栏目"> </el-table-column>
<el-table-column label="操作">
    <template v-slot="slot">
        <a herf='' @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
        <a herf='' @click.prevent="toupdateHandler(slot.row)">修改</a>
    </template>
</el-table-column>
</el-table>
<!--/表格分页-->
<el-pagination
    layout="prev, pager, next"
    :total="50">
  </el-pagination>
  <!--模态框-->
  <el-dialog
  :title="title"
  :visible.sync="visible"
  width="40%">
  ----{{form}} 

 <el-form :model="form" label-width="80px" label-position="top">
     <el-form-item label="栏目名称">
         <el-input v-model="form.name"></el-input>

     </el-form-item>
       <el-form-item label="序号" >
         <el-input  v-model="form.num"></el-input>

     </el-form-item>
 </el-form>
  <span slot="footer" class="dialog-footer">
    <el-button size="small" @click="closeModalHandler">取 消</el-button>
    <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
  </span>
</el-dialog>

</div>

</template>
      
<script>//暴露接口
import request from'@/utils/request'
import querystring from'querystring'//序列化
export default {
    //用于存放网页中需要的方法
    methods:{
        
        
        lodaData(){
            let url="http://localhost:6677/category/findAll"
            request.get(url).then((response)=>{
            //将查询到的值放到customers里面,外部this指向内部this
            this.categorys=response.data;
})
        },
        submitHandler(){
            //通过reuqest与后台交互
            //this.from  对象--》字符串--->后台
            let url="http://localhost:6677//category/saveOrUpdate"
            request({
                url,
                method:"POST",
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
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
        toDeleteHandler(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        })},
   toAddHandler(){
            this.title="录入栏目信息";
             this.visible=true;
        },
         toupdateHandler(){
            this.title="修改栏目信息";
             this.visible=true;
        },
        
        closeModalHandler(){
             this.visible=false;
        }
    },
    //存放前端数据
    data(){
      
        return{
            title:"添加栏目信息",
            visible:false,
        categorys:[{}],
        form:{
             type:"categorys"
        }
        }
    },
    created(){
//vue实例加载完毕
this.lodaData();
    }
}
</script>

<style scoped>

</style>