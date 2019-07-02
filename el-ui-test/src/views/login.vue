<template>
    <el-form :model="ruleForm2" :rules="rules" label-position="left" label-width="auto" ref="ruleForm2" class="form-container">
        <h3 class="title">系统登录</h3>
        <el-form-item prop="account" label="用户名">
            <el-input type="text" placeholder="输入用户名" v-model="ruleForm2.account"></el-input>
        </el-form-item>
        <el-form-item prop="checkPass" label="密码">
            <el-input type="password" placeholder="输入密码" v-model="ruleForm2.checkPass"></el-input>
        </el-form-item>
        <el-form-item style="text-align:left;">
            <el-checkbox v-model="checked" checked>记住密码</el-checkbox>
        </el-form-item>
        <el-form-item style="width:100%;">
            <el-button style="width:100%;" type="primary" @click="login" :loading="loading">登录</el-button>
        </el-form-item>
        
    </el-form>
</template>

<script>
import { setTimeout } from 'timers';
export default {
    name: "login",
    data () {
        return {
            ruleForm2: {
                account: 'admin',
                checkPass: '123456'
            },
            checked: true,
            rules: {
                account: [
                    {required: true, message: "用户名不能为空", trigger: "blur"}
                ],
                checkPass: [
                    {required: true, message: "密码不能为空", trigger: "blur"}
                ]
            },
            loading: false
        }
    },
    methods: {
        login () {
            this.loading = true
            setTimeout(() => {
                this.loading = false
                this.$router.push("/table")
            },1000)
        }
    }
}
</script>

<style lang="scss" scoped>
.form-container{
    width: 350px;
    margin: 180px auto;
    padding: 20px;
    border-radius: 10px;
    border: 1px solid skyblue;
}
.title {
    font-size: 20px;
    line-height: 20px;
    text-align: center;
    margin-bottom: 20px;
}
</style>
