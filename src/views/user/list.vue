<template>
  <div>
      <el-table
        :data="tableData.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
        style="width: 100%">
        <el-table-column
        label="Date"
        prop="date">
        </el-table-column>
        <el-table-column
        label="Name"
        prop="name">
        </el-table-column>
        <el-table-column
        label="address"
        prop="address">
        </el-table-column>
        <el-table-column
        align="right">
        <template slot="header" slot-scope="scope">
          <el-input
            v-model="search"
            size="mini"
            placeholder="输入关键字搜索"/>
        </template>
        <template slot-scope="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)">Edit</el-button>
        <el-button
          size="mini"
          type="danger"
          @click="handleDelete(scope.$index, scope.row)">Delete</el-button>
        </template>
        </el-table-column>
      </el-table>
      <el-dialog
        title="提示"
        :visible.sync="dialogVisible"
        width="50%"
        :before-close="handleClose">
        <el-form :model="current" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
         <el-form-item label="日期" prop="date">
          <el-date-picker
            v-model="current.date"
            type="date"
            format="yyyy-MM-dd"
            value-format="yyyy-MM-dd"
            placeholder="选择日期">
          </el-date-picker>
        </el-form-item>
          <el-form-item label="姓名" prop="name">
            <el-input type="text" v-model="current.name" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="地址" prop="address">
            <el-input v-model.number="current.address"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="submitForm()">提交</el-button>
            <el-button @click="handleClose">取消</el-button>
          </el-form-item>
        </el-form>
      </el-dialog>
  </div>
</template>

<script>
  export default {
    data() {
      const dateValidate = (ruler, value, callback)=>{
        console.log('value...', value);
        if (value < '2018-01-01'){
          callback(new Error('当前日期不能小于2018-01-01'))
        }else{
          callback();
        }
      }
      return {
        tableData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'                 
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }],
        search: '',
        current:{},
        rules:{
          date: [{trigger:'blur', validator: dateValidate}],
          name:[{trigger:'blur',required:'true'}],
          address:[{trigger:'blur',required:'true'}]
        },
        dialogVisible:false
      }
    },
    methods: {
      handleEdit(index, row) {
        this.dialogVisible = true;
        this.current = row;
        console.log(index, row);
      },
      handleDelete(index, row) {
        console.log(index, row);
      },
      handleClose(){
        this.dialogVisible = false
      },
      submitForm(){
        this.$refs.ruleForm.validate(valid=>{
          if(valid){
            console.log('校验通过 ');
            this.$notify({
              title:'成功',
              message:'修改成功',
              type:'success'
            })
          }else{
            this.$notify({
              title:'失败',
              message:'修改失败',
              type:'error'
            })
          }
        })
        console.log(this.current)
      }
    },
  }
</script>