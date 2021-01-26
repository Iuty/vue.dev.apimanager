<template>
    <div class="namespace">
        <el-row class="namespace-box">
        <el-col :span="4" @click.native="showChange">
            <i class="el-icon-arrow-down" v-if="show"></i>
            <i class="el-icon-arrow-right" v-else></i>
        </el-col>
        <el-col :span="18" @click.native="showChange">
            <i>{{title}}</i>
        </el-col>
        
        </el-row>  
        <el-row v-for="(api,index) in apis" v-show="show" class="request">
            <el-col :span="18">
                <a @click="onSelectApi(api.aid,api.name)">{{api.name}}</a> 
            </el-col>  
            <el-col :span="6">
                <el-button class="el-icon-edit" @click.native="update(api)"></el-button>
            </el-col>
        </el-row>
        <apiinfo ref="apiinfo" @commited="onUpdated"></apiinfo>
    </div>
</template>

<script>
    import apiinfo from '../../form/apiinfo.vue'

    export default{
        name:"namespace",
        data:function(){
            return {
                
                show:false
            }
        },
        components:{
           apiinfo 
        },
        props:{
            
            title:{
                type:String,
                default:'test namespace'
            },
            apis:{
                type:Array,
                default:[]
            }
        },
        methods:{
            showChange:function(){
                
                this.show = !this.show
                this.$forceUpdate()
            },
            onSelectApi:function(aid,name){
                
                this.$emit("selected",aid,name)
            },
            onUpdated:function(aid,name){
                
                this.$emit("updated",aid,name)
            },
            update:function(api){
                let form = this.$refs.apiinfo
                
                form.show(api)
            }
        }
    }
</script>

<style>
    .namespace{
        padding-top: 1px;
        background-color: #ccc;
    }
    .namespace-box{
        border: 1px solid #2C3E50;
        font-size: 20px;
        vertical-align: middle;
        width: 100%;
        background-color: #ddd;
    }
    
    .request{
        background-color: #eee;
        font-size: 20px;
    }
</style>
