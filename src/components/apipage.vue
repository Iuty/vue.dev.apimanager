<template>
    <div class="apipage">
        <el-row ref="info" class="api-info">
            <el-col :span="6" :offset="2">
                <i>{{info.namespace}} / {{info.name}}</i>
            </el-col >
            
            <el-col :span="8" :offset="2">
                <i>[{{info.description}}]</i>
                
            </el-col>            
        </el-row>
        </br>
        <el-row>            
            <el-col :span="20" :offset="2">
                <el-input disabled=""  v-model="info.url">
                    <template slot="prepend">Url:</template>
                </el-input>
            </el-col>
            
        </el-row>
        
        <hr></hr>
        <el-row class="api-param">
            <el-col :span="12">
                <el-row>
                    <el-col :span="4">
                        <h3>请求输入:</h3>
                    </el-col>
                    <el-col :span="4" :offset="16" class="api-param-add">
                        <el-button @click.native="updateParam(0,'0')">+</el-button>
                    </el-col>
                </el-row>
                <el-row v-for="(v,i) in param['0']">
                    <el-col :span="6">
                        <el-input placeholder="key" disabled 
                        v-model="v.key"></el-input>
                    </el-col>
                    <el-col :span="14">
                        <el-input placeholder="description" disabled
                        v-model="v.description"></el-input>
                    </el-col>
                    <el-col :span="2">
                        <el-button @click.native="updateParam(v.apid,'0')"><i class="el-icon-edit"></i></el-button>
                    </el-col>
                    
                </el-row>
            </el-col>
            <el-col :span="12">
                <el-row>
                    <el-col :span="4">
                        <h3>请求输出:</h3>
                    </el-col>
                    <el-col :span="4" :offset="16" class="api-param-add">
                        <el-button @click.native="updateParam(0,'1')">+</el-button>
                    </el-col>
                </el-row>
                <el-row v-for="(v,i) in param['1']">
                    <el-col :span="6">
                        <el-input placeholder="key" disabled 
                        v-model="v.key"></el-input>
                    </el-col>
                    <el-col :span="14">
                        <el-input placeholder="description" disabled
                        v-model="v.description"></el-input>
                    </el-col>
                    <el-col :span="2">
                        <el-button @click.native="updateParam(v.apid,'1')"><i class="el-icon-edit"></i></el-button>
                    </el-col>
                </el-row>
                
            </el-col>
        </el-row>
        <hr></hr>
        <el-row ref="example" class="api-example">
            <el-col :span="8"
            v-for="(e,i) in example">
                <el-col :span="12" :offset="2" class="example-name">
                    <i>{{e.name}}</i>
                </el-col>
                <el-col :span="4" :offset="4">
                    <el-button @click.native="showExample(e.eid,e)">测试</el-button>
                </el-col>
            </el-col>
            <el-col :span="8">
                <el-col :span="12" :offset="2" class="example-name">
                    <i>new example</i>
                </el-col>
                <el-col :span="4" :offset="4">
                    <el-button @click.native="showExample(0,aid)" type="warning">新增</el-button>
                </el-col>
            </el-col>
        </el-row>
        
        <apiparam ref="apiparam" @updated="getApiParam"></apiparam>
        <example ref="example" @updated="getExample"></example>
    </div>
</template>

<script>
    import $ from 'jquery';
    import apiparam from "../../form/apiparam.vue"
    import example from "../../form/example.vue"
    export default{
        name:'apipage',
        data:function(){
            return {
                mode:"release",
                info:{
                    name:"new api",
                    namespace:"default namespace",
                    url:"",
                    debugurl:"",
                    description:""
                },
                param:{
                    "0":[],
                    "1":[],
                },
                example:[]
            }
        },
        props:{
            aid:{
                type:String,
                default:"0"
            }
        },
        components:{
            apiparam,
            example
        },
        mounted:function(){
            this.getApiInfo();
            this.getApiParam();
            this.getExample();
        },
        
        methods:{
            getApiParam:function(){
                let _ = this;
                var settings = {
                  "url": "http://www.iutyservice.xyz/api/dev/api",
                  "method": "POST",
                  
                  "data": {
                    "cmd": "getApiParam",
                    "aid": _.aid
                  }
                };
                
                $.ajax(settings).done(function (response) {
                    if(response.success){
                        let data = response.data;
                        _.param['0'] = [];
                        _.param['1'] = [];
                        if(null != data["0"]){
                            _.param['0'] = data['0']
                        }
                        if(null != data["1"]){
                            _.param['1'] = data['1']
                        }
                        _.$forceUpdate()
                    }
                });
            },
            getExample:function(){
                let _ = this
                var settings = {
                  "url": "http://www.iutyservice.xyz/api/dev/api",
                  "method": "POST",
                  
                  "data": {
                    "cmd": "getExample",
                    "aid": _.aid,
                    
                  }
                };
                
                $.ajax(settings).done(function (response) {
                  if(response.success){
                      _.example = response.data
                      _.$refs.example.$forceUpdate()
                  }
                });
            },
            getApiInfo:function(){
                let _ = this
                var settings = {
                  "url": "http://www.iutyservice.xyz/api/dev/api",
                  "method": "POST",
                  
                  "data": {
                    "cmd": "getApiInfo",
                    "aid": _.aid
                  }
                };
                
                $.ajax(settings).done(function (response) {
                  
                  if(response.success){
                      
                      let data = response.data
                      
                      if(data.length>0){
                          _.info = data[0];
                          _.$forceUpdate()
                          
                      }
                  }
                });
            },
            updateParam:function(apid,dir){
                let param = {
                    apid:apid,
                    aid:this.aid,
                    description:"",
                    key:"",
                    direction:Number(dir)
                }
                if(apid != 0){
                    for(let item in this.param[dir]){
                        
                        if(this.param[dir][item].apid == apid){
                            param = this.param[dir][item]
                            break
                        }
                    }
                }
                
                this.$refs.apiparam.show(param)
            },
            showExample:function(eid,example){
                
                this.$refs.example.show(eid,example);
            }
        }
    }
</script>

<style>
    
    .api-info{
        font-size: 30px;
        text-align: left;
        padding-left: 5px;
    }
    .api-param{
        text-align: left;
    }
    .api-param-add{
        padding-top: 10px;
    }
    .example-name{
        font-size: 20px;
        padding-top: 3px;
    }
    .api-example-title{
        text-align: left;
        padding-left: 5px;
    }
    .api-example-param{
        text-align: -webkit-center;
        
    }
</style>
