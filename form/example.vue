<template>
    <el-dialog
    title="测试用例"
    :visible.sync="visible"
    width="60%">
        <el-row class="example-info">
            <el-row>
                <el-col :span="2" :offset="1">
                    <el-button type="danger" @click.native="deleteExample" :disabled="eid==0">删除</el-button>
                    
                </el-col>
                <el-col :span="2" :push="17">
                    <el-button type="primary" @click.native="commit">保存</el-button>
                </el-col>
            </el-row>
            </br>
            <el-row>
                <el-col :span="4" :offset="1">
                    <el-input v-model="info.name" placeholder="example name here"></el-input>
                </el-col>
                <el-col :span="16" :offset="1">
                    <el-input v-model="info.description" placeholder="example description here"></el-input>
                </el-col>
            </el-row>
        </el-row>
        <hr></hr>
        <el-row>
            <el-col :span="2" :offset="1">
                <el-button type="primary" @click.native="getJQCode" :disabled="eid==0">JQ代码</el-button>
            </el-col>
            <el-col :span="2" :offset="14">
                <el-button type="primary" @click.native="saveParam" :disabled="eid==0">保存</el-button>
            </el-col>
            
            <el-col :span="2" :offset="1">
                <el-button type="success" @click.native="sendRequest" :disabled="eid==0">发送</el-button>
            </el-col>
        </el-row>
        <br>
        <el-row class="example-param"
        v-for="(p,i) in param">
            <el-col :span="1" :offset="1" class="example-param-enabled">
                <el-checkbox v-model="p.enabled"></el-checkbox>
            </el-col>
            <el-col :span="4" :offset="1">
                <el-input disabled v-model="p.key"></el-input>
            </el-col>
            <el-col :span="4" :offset="1">
                <el-input placeholder="value here" v-model="p.value"></el-input>
            </el-col>
            <el-col :span="10" :offset="1">
                <el-input disabled v-model="p.description"></el-input>
            </el-col>
        </el-row>
        <hr>
        <el-row ref="response">
            <el-input type="textarea" :rows="12" v-model="responsetext" :disabled="eid==0"></el-input>
        </el-row>
    </el-dialog>
</template>

<script>
    import $ from 'jquery'
    export default{
        name:'example',
        data:function(){
            return {
            visible:false,
            info:{
                description:"",
                name:""
            },
            param:[],
            responsetext:"",
            eid:0,
            
            }
        },
        
        methods:{
            show:function(eid,example){
                this.eid = eid
                if(eid != 0){
                    
                    $.extend(this.info,example)
                    
                }
                else{
                    this.info={
                        eid:0,
                        aid:example,
                        description:"",
                        name:""
                    }
                }
                this.getParam()
                this.responsetext = "";
                this.visible = true
            },
            getParam:function(){
                let _ = this
                var settings = {
                  "url": "http://www.iutyservice.xyz/api/dev/api",
                  "method": "POST",
                  
                  "data": {
                    "cmd": "getExampleParam",
                    "eid": _.eid
                  }
                };
                
                $.ajax(settings).done(function (response) {
                  if(response.success){
                      _.param = response.data;
                      _.$refs.response.$forceUpdate()
                  }
                });
            },
            deleteExample:function(){
                
                let _ = this;
                let settings =  {
                  "url": "http://www.iutyservice.xyz/api/dev/api",
                  "method": "POST",
                  "data": {
                    "cmd": "deleteExample",
                    "eid": _.eid
                  }
                };
                
                
                $.ajax(settings).done(function(response){
                    _.$emit("updated")
                    _.visible = false
                });
            },
            commit:function(){
                let _ = this
                var settings = {
                  "url": "http://www.iutyservice.xyz/api/dev/api",
                  "method": "POST",
                  
                  "data": {
                    "cmd": "updateExample",
                    "aid": _.info.aid,
                    "name": _.info.name,
                    "description": _.info.description,
                    "eid": _.info.eid
                  }
                };
                
                if(this.eid==0){
                    this.visible = false;
                }
                $.ajax(settings).done(function (response) {
                    _.$emit("updated")
                });
            },
            saveParam:function(){
                let _ = this
                for(let p in _.param){
                    var settings = {
                      "url": "http://www.iutyservice.xyz/api/dev/api",
                      "method": "POST",
                      
                      "data": {
                        "cmd": "updateExampleParam",
                        "apid": _.param[p].apid,
                        "eid": _.info.eid,
                        
                        "value": _.param[p].value,
                        
                      }
                    };
                    if( _.param[p].enabled){
                        settings.data.enabled = true;
                    }
                    $.ajax(settings).done(function (response) {
                        
                        if(!response.success){
                            _.message({
                                message:response.error,
                                type:"danger"
                            })
                        }
                    });
                }
            },
            sendRequest:function(){
                let _ = this;
                var settings = {
                  "url": _.info.url,
                  "method": "POST",
                  
                  "data": {
                      
                  }
                };
                for(let p in _.param){
                    let pa = _.param[p]
                    if(pa.enabled){
                        settings.data[pa.key] = pa.value
                    }
                }
                
                $.ajax(settings).done(function (response) {
                    
                    _.responsetext = JSON.stringify(response,null,4)
                    _.$refs.response.$forceUpdate()
                });
            },
            getJQCode:function(){
                let _ = this;
                var settings = {
                  "url": _.info.url,
                  "method": "POST",
                  
                  "data": {
                      
                  }
                };
                for(let p in _.param){
                    let pa = _.param[p]
                    if(pa.enabled){
                        settings.data[pa.key] = pa.value
                    }
                }
                
                let jqcode = "let _ = this;\r\nlet settings =  "+JSON.stringify(settings,null,2) + ";\r\n$.ajax(settings).done(function(response){\r\n\r\n});"
                _.responsetext = jqcode;
                _.$refs.response.$forceUpdate()
            }
        }
    }
</script>

<style>
    .example-param-enabled{
        padding-top:8px;
    }
</style>
