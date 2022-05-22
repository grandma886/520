<template>
  <div class="app-container" >
    <switch-roles @change="handleRolesChange" />
    <div>
      <div v-permission="['admin']"style="margin-top:80px;">
        <input type="text">
        <select v-model="goodsId" style="width:200px;height: 30px;margin-top: 5px;margin-bottom: 5px;margin-left: 10px">
          <option value="">请选择</option>
          <option v-for="item in kindList":key="i" v-bind:value="item.id" v-text="item.name" ></option>
        </select>
      </div>
      <div  v-permission="['admin','editor']" style="margin-top:30px;">
        <list-layout>
            <template slot="header">
                <conditions />
            </template>
            <template slot="content">
                <al-table :column="formData.column" :data="formData.list" :pagination="true">
                </al-table>
            </template>
        </list-layout>
      </div>
    </div> 
  </div>
</template>

<script>
import permission from '@/directive/permission/index.js' // 权限判断指令
import checkPermission from '@/utils/permission' // 权限判断函数
import SwitchRoles from './components/SwitchRoles'

import { getTestList } from "@/api/test.js";
import ListLayout from "@/components/ListLayout/index.vue";
import AlTable from "@/components/AlTable/index.vue";
import data from '../pdf/content';
import Conditions from "../test/compponents/conditions/index.vue";

export default {
  name: 'DirectivePermission',
  components: { SwitchRoles },
  directives: { permission },
  data() {
    return {
      key: 1 ,// 为了能每次切换权限的时候重新初始化指令
      kindList:[{name:"阿猫",id:1},{name:"阿狗",id:2},{name:"小宝龙",id:3}],
      formData:{
                    column:[
                        {
                           prop:'nick',
                           label:'昵称', 
                        },
                        {
                           prop:'phone',
                           label:'手机号', 
                        },
                        {
                           prop:'name',
                           label:'姓名', 
                        },
                        {
                           prop:'subjet',
                           label:'所属学科', 
                        },
                        {
                           prop:'job',
                           label:'职位', 
                        },
                        {
                           prop:'year',
                           label:'工作年限', 
                        },
                        {
                           prop:'updateTime',
                           label:'录入时间', 
                        },
                        {
                           prop:'action',
                           label:'操作', 
                           render(_,scope){
                               return(
                                   <div>
                                   <el-button>操作</el-button></div>
                               )
                           }
                        },
                    ],
                    list:[]
                }
    }
  },
        components:{
            ListLayout,
            AlTable,
            Conditions
        },
        created () {
            getTestList().then((res)=>{
                const{code,data}=res;
                if(Number(code)===20000){
                    const{items,total}=data
                    this.formData.list=items
                }
            })
        },
  methods: {
    checkPermission,
    handleRolesChange() {
      this.key++
    }
  }
}
</script>

<style lang="scss" scoped>
.app-container {
  ::v-deep .permission-alert {
    width: 320px;
    margin-top: 15px;
    background-color: #f0f9eb;
    color: #67c23a;
    padding: 8px 16px;
    border-radius: 4px;
    display: inline-block;
  }
  ::v-deep .permission-sourceCode {
    margin-left: 15px;
  }
  ::v-deep .permission-tag {
    background-color: #ecf5ff;
  }
}
</style>

