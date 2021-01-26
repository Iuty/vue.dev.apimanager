<template>
    <el-dialog 
    title="新增/修改接口信息"
    :visible.sync="visible"
    width="60%">
        
        <el-row class="form-apiinfo1">
            <el-col :span="4" :offset="2">
                <el-input placeholder="namespace here" v-model="formdata.namespace"></el-input>
            </el-col >
            <el-col :span="1" class="divide">
                <i>/</i>
            </el-col>
            <el-col :span="4">
                <el-input placeholder="api name here" v-model="formdata.name"></el-input>
            </el-col>
            <el-col :span="10" :offset="1">
                <el-input placeholder="description here" v-model="formdata.description"></el-input>
            </el-col>
            
        </el-row>
        
        <el-row class="form-apiinfo-2">            
            <el-col :span="20" :offset="2">
                <el-input placeholder="e.g. http://www.example.com/api" v-model="formdata.url"></el-input>
            </el-col>
            
        </el-row>
        </br>
        </br>
            <el-row>
                
                <el-col :span = "4" :offset="10">
                    <el-button type="primary" @click="commit"> 提 交 </el-button>
                </el-col>
                <el-col :span = "2" :offset="6">
                    <el-button type="danger" @click.native="deleteApiInfo" :disabled="disdelete">
                        <i class="el-icon-delete"></i>
                    </el-button>
                </el-col>
                
            </el-row>
            
         
    </el-dialog>
</template>

<script>
    import $ from 'jquery'
    export default{
        name:"apiinfo",
        props:{
            disdelete:{
                type:Boolean,
                default:false
            }
        },
        data:function(){
            return {
                visible:false,
                formdata:{
                    aid:"*0",
                    namespace:"",
                    name:"",
                    description:"",
                    url:"http://www.iutyservice.xyz/api"
                },
                
            }
        },
        methods:{
            show:function(formdata){
                $.extend(this.formdata,formdata)                
                this.visible = true
            },
            commit:function(){
                this.updateApiInfo();
                this.visible = false
            },
            updateApiInfo:function(){
                let _ = this
                var settings = {
                    "url": "http://www.iutyservice.xyz/api/dev/api",
                    "method": "POST",
                    
                    "data": {
                      "cmd": "updateApiInfo",
                      "aid": _.formdata.aid,
                      "name": _.formdata.name,
                      "namespace": _.formdata.namespace,
                      "url": _.formdata.url,
                      "description": _.formdata.description,
                      
                    }
                  };
                  
                  $.ajax(settings).done(function (response) {
                    
                    if(response.success){
                        
                        let data = response.data 
                        
                        _.$emit("commited",data[0].aid,data[0].name);
                    }
                    else{
                        _.$message({
                            message:response.error,
                            type:"error"
                        })
                    }
                  });
            },
            deleteApiInfo:function(){
                let _ = this;
                let settings =  {
                  "url": "http://www.iutyservice.xyz/api/dev/api",
                  "method": "POST",
                  "data": {
                    "cmd": "deleteApiInfo",
                    "aid": _.formdata.aid
                  }
                };
                
                $.ajax(settings).done(function(response){
                    
                    _.$emit("commited")
                    _.visible = false;
                });
            }
        }
    }
</script>

<style>
    .form-apiinfo-1{
        padding:1px;
    }
    .form-apiinfo-2{
        padding:1px;
    }
    .divide{
        font-size: 25px;
    }
</style>
