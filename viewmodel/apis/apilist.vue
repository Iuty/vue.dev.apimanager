<template>
    <div>
        <el-row>
            <el-col :span='18'>
                <el-input
                    placeholder="请输入内容"
                    prefix-icon="el-icon-search"
                    v-model="search">
                </el-input>
            </el-col>
            <el-col :span='6'>
                <el-button @click="addApi">+</el-button>  
            </el-col>
            
        </el-row>
        
        <namespace v-for="(api,key,index) in apis" :title='key' :apis="api" 
        @selected="onSelectApi"
        @updated="onUpdated"></namespace>
        <apiinfo ref="apiinfo" @commited="onUpdated" :disdelete="true"></apiinfo>
    </div>
</template>

<script>
    import $ from 'jquery'
    
    import namespace from '../../src/components/namespace.vue'
    import apiinfo from '../../form/apiinfo.vue'
    export default {
        
        data:function(){
          return {
              apis:[],
              selectaid : 0,
              search:""
          }  
        },
        methods: {
            getApis:function(){
                let _ = this
                var settings = {
                  "url": "http://www.iutyservice.xyz/api/dev/api",
                  "method": "POST",
                  
                  "data": {
                    "cmd": "getApis"
                  }
                };
                
                $.ajax(settings).done(function (response) {
                  
                  if(response.success){
                      _.apis = response.data
                      
                      _.$forceUpdate()
                  }
                });
            },
            onAdded:function(){
                this.getApis();
                
            },
            onUpdated:function(aid,name){
                
                this.getApis();
                
                this.$emit("updated",aid,name)
            },
            onSelect:function(aid){
                
                this.selectaid = aid
            },
            onSelectApi:function(aid,name){
                
                this.$emit("selected",aid,name)
                
            },
            addApi:function(){
                
                let form=this.$refs.apiinfo
                let api={
                    aid:"*0",
                    namespace:"",
                    name:"",
                    description:"",
                    url:"http://iutyservice.xyz/api"
                }
                form.show(api)
            }
            
        },
        components:{
            namespace,
            apiinfo
        },
        mounted:function(){
            this.getApis();
        }
      };
    
</script>

<style>
</style>
