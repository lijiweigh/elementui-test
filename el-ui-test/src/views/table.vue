<template>
    <el-row class="container">
        <el-row class="header"  :gutter="20">
            <el-col :span="10" style="width: 230px;">VUEADMIN</el-col>
            <el-col :span="10">
                <div class="tools">
                    hahaha
                </div>
            </el-col>
            <el-col :span="4" style="float:right">
                <el-dropdown @command="handleCommand"  trigger="hover">
                     <span class="avatar"></span>
                     
                     <template v-slot:dropdown>
                          <el-dropdown-menu>
                            <el-dropdown-item command="a">我的消息</el-dropdown-item>
                            <el-dropdown-item command="b">设置</el-dropdown-item>
                            <el-dropdown-item command="c" divided>退出</el-dropdown-item>
                        </el-dropdown-menu>
                     </template>
                   
                   
                </el-dropdown>

                
            </el-col>
        </el-row>
    </el-row>
</template>

<script>
export default {
    name: "mytable",
    methods: {
        handleCommand (command) {
            console.log(command)
        }
    }
}
</script>


<style lang="scss" scoped>
.header {
    // width: 100%;
    height: 60px;
    background: skyblue;
    line-height: 60px;
    color: #fff;
}

.avatar {
    display: inline-block;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    margin-top: 5px;
    background: pink;
}
</style>

