<template>
    <el-container class="container">
        <el-header class="header" style="padding:0;">
            <el-row>
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
        </el-header>
        <el-container>
             <el-aside height="100%"  style="background-color:#eee;">
                 <el-menu @select="handleselect" router  height="100%" style="background-color:#eee;">
                     <template v-for="(menu, index) in menus">
                         <el-submenu :key="index" :index="menu.index" v-if="menu.children">
                            <template #title>{{menu.title}}</template>
                            <el-menu-item v-for="(item, index) in menu.children" :key="index" :index="item.index">{{item.title}}</el-menu-item>
                        </el-submenu>
                        <el-menu-item v-else :key="index" :index="menu.index">{{menu.title}}</el-menu-item>
                     </template>
                 </el-menu>
             </el-aside>
             <el-main style="padding:0;">
                 <el-row>
                     <el-col :span="4">
                         <strong>{{$route.name}}</strong>
                     </el-col>
                     <el-col :offset="16" :span="4">
                         <el-breadcrumb separator="/">
                            <el-breadcrumb-item v-for="(item, index) in $route.matched" :key="index">{{item.name}}</el-breadcrumb-item>
                         </el-breadcrumb>
                     </el-col>
                 </el-row>
                 <router-view></router-view>
             </el-main>
        </el-container>
    </el-container>
</template>

<script>
export default {
    name: "el-index",
    data () {
        return {
            menus: [
                {
                    index: "/nav1",
                    title: "导航一",
                    children: [
                        {
                            index: "/table",
                            title: "table"
                        },
                        {
                            index: "/form",
                            title: "form"
                        },
                        {
                            index: "/list",
                            title: "list"
                        }
                    ]
                },
                {
                    index: "/nav2",
                    title: "导航二",
                    children: [
                        {
                            index: "/page4",
                            title: "page4"
                        },
                        {
                            index: "/page5",
                            title: "page5"
                        }
                    ]
                },
                {
                    index: "/page6",
                    title: "导航三"
                },
                {
                    index: "/nav3",
                    title: "charts",
                    children: [
                        {
                            index: "/charts",
                            title: "charts"
                        }
                    ]
                }
            ],
            
        }
    },
    methods: {
        handleCommand (command) {
            console.log(command)
        },
        handleselect (index, pathIndex) {
            
            console.log(index, pathIndex)
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

