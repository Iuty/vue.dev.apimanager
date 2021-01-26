<template>
    <el-dialog 
    title="新增/修改接口参数"
    :visible.sync="visible"
    width="60%">
        <el-row>
            <el-col :span="3">
                <el-select v-model="formdata.direction" placeholder="请选择">
                    <el-option 
                      
                      label="输入"
                      :value='0'>
                    </el-option>
                    <el-option 
                      
                      label="输出"
                      :value='1'>  
                    </el-option>
                  </el-select>
            </el-col>
            <el-col :span="5" :offset="1">
                <el-input placeholder="api param name here" v-model="formdata.key"></el-input>
            </el-col>
            
            <el-col :span="13" :offset="1">
                <el-input placeholder="description here" v-model="formdata.description"></el-input>
            </el-col>
        </el-row>
        </br>
        <el-row>
            <el-col :span="2" :offset="1">
                <el-button @click.native="deleteApiParam" type="danger" :disabled="formdata.apid==0">删除</el-button>
            </el-col>
            <el-col :span="2" :offset="9">
                <el-button @click.native="commit" type="primary">提交</el-button>
            </el-col>
        </el-row>
    </el-dialog>
</template>

<script>
    import $ from 'jquery'
    export default{
        name:"apiparam",
        data:function(){
            return{
                visible:false,
                formdata:{
                    key:"",
                    description:"",
                    direction:0
                }
            }
        },
        methods:{
            show:function(param){
                $.extend(this.formdata,param);
                
                this.visible = true
            },
            
            commit:function(){
                
                let _ = this
                
                var settings = {
                  "url": "http://www.iutyservice.xyz/api/dev/api",
                  "method": "POST",
                  
                  "data": {
                    "cmd": "updateApiParam",
                    "aid": _.formdata.aid,
                    "description": _.formdata.description,
                    "apid": _.formdata.apid,
                    "key": _.formdata.key,
                    "direction": _.formdata.direction
                  }
                };
                
                $.ajax(settings).done(function (response) {
                    
                    
                    _.$emit("updated")
                });
                _.visible = false
            },
            deleteApiParam:function(){
                let _ = this;
                let settings =  {
                  "url": "http://www.iutyservice.xyz/api/dev/api",
                  "method": "POST",
                  "data": {
                    "cmd": "deleteApiParam",
                    "apid": _.formdata.apid
                  }
                };
                $.ajax(settings).done(function(response){
                    _.$emit("updated")
                    
                });
                _.visible = false
            }
        }
    }
</script>

<style>
</style>
