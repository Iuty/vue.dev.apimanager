<template>
    
    <div>
        <el-col :span='5' class="apimgr">            
            <apilist ref="apilist" @selected="onSelectedApi" @updated="onUpdateApi"></apilist>            
        </el-col>
        <el-col :span='19' class="apiinfo">
            <el-tabs ref="apiinfo" v-model="selectedtab" type="card" closable @tab-remove="onCloseTab">
                <el-tab-pane v-for="(t,i) in tabkeys" :name="t" :label="tablenames[i]">
                    <apipage :ref="'aid:'+t" :aid="t" ></apipage>
                </el-tab-pane>
            </el-tabs>
        </el-col>
    </div>
    
</template>

<script>
    import apilist from './apis/apilist.vue'
    import apipage from '../src/components/apipage.vue'
    export default{
        components:{
            apilist,
            apipage
        },
        data:function(){
            return {
                tabkeys:[],
                tablenames:[],
                selectedtab:0
            }
        },
        methods:{
            onSelectedApi:function(aid,name){                
                let tabkey = aid.toString()
                let index = this.tabkeys.indexOf(tabkey)
                if(-1 == index){
                    this.tabkeys.push(tabkey)
                    this.tablenames.push(name)
                    index = this.tabkeys.length-1;
                }
                else{
                    if(this.tablenames[index] != name){
                        this.tablenames[index] = name;
                        
                    }
                    
                }
                
                
                this.selectedtab = tabkey;
                this.$refs.apiinfo.$forceUpdate();
                
                //this.$forceUpdate()
                
            },
            onUpdateApi:function(aid,name){
                let tabkey = "aid:"+aid
                //alert(this.$refs.tabkey)
                this.onCloseTab(aid)
                //this.$refs[aid].$forceUpdate();
                //this.$refs[tabkey].$forceUpdate();
                
                
            },
            
            onCloseTab:function(n){
                let tabkey = n.toString()
                let index = this.tabkeys.indexOf(tabkey)
                
                
                this.tablenames.splice(index,1)
                this.tabkeys.splice(index,1)
                
                if (this.selectedtab == tabkey){
                    
                    if(this.tabkeys.length > 0){
                        
                        this.selectedtab = this.tabkeys[0];
                    }
                    else{
                        
                        this.selectedtab = null
                    }
                }
                
                this.$refs.apiinfo.$forceUpdate();
            },
            refresh:function(){                
                this.$refs.apilist.getApis();
            }
        }
    }
</script>

<style>
    .apiinfo{
        padding:1px;
        background-color: floralwhite;
    }
    .apimgr{
        padding-left: 2px;
        
    }
</style>
