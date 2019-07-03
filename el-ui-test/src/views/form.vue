<template>
    <div class="form">
        <el-form :model="formData" label-position="left" label-width="auto">
            <el-form-item label="活动名称" prop="name">
                <el-input v-model="formData.name"></el-input>
            </el-form-item>
            <el-form-item label="活动区域" prop="region">
                <el-select v-model="formData.region">
                    <el-option label="区域一" value="北京"></el-option>
                    <el-option label="区域二" value="上海"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="活动时间">
                <el-row>
                    <el-col :span="10">
                        <el-form-item prop="date">
                            <el-date-picker value-format="yyyy-MM-dd" format="yyyy-MM-dd" v-model="formData.date"></el-date-picker>
                        </el-form-item>
                        
                    </el-col>
                    <el-col :span="10" :offset="4">
                        <el-form-item prop="time">
                            <el-time-picker v-model="formData.time"></el-time-picker>
                        </el-form-item>
                    </el-col>
                </el-row>
            </el-form-item>
            <el-form-item label="即时配送" prop="immediate">
                <el-switch v-model="formData.immediate"></el-switch>
            </el-form-item>
            <el-form-item label="活动性质" prop="xingzhi">
                <el-checkbox :indeterminate="is" v-model="checkAll" @change="indeChange">全选</el-checkbox>
                <el-checkbox-group v-model="formData.xingzhi">
                    <el-checkbox label="性质1"></el-checkbox>
                    <el-checkbox label="性质2"></el-checkbox>
                    <el-checkbox label="性质3"></el-checkbox>
                    <el-checkbox label="性质4"></el-checkbox>
                    <el-checkbox label="性质5"></el-checkbox>
                    <el-checkbox label="性质6"></el-checkbox>
                </el-checkbox-group>
            </el-form-item>
            <el-form-item label="活动资源" prop="special">
                <el-radio-group v-model="formData.special">
                    <el-radio label="资源一"></el-radio>
                    <el-radio label="资源二"></el-radio>
                    <el-radio label="资源三"></el-radio>
                </el-radio-group>
            </el-form-item>
            <el-form-item label="活动形式" prop="xingshi">
                <el-input type="textarea" autosize resize="none" v-model="formData.xingshi"></el-input>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
let options = ["性质1","性质2","性质3","性质4","性质5","性质6"]
export default {
    name: "myform",
    data () {
        return {
            formData: {name: "", region: "",date: "", time: "",immediate: false, xingzhi: ["性质一","性质二"], special: "",xingshi: ""},
            checkAll: false,
            is: true,
            radioGroup: ""
        }
    },
    methods: {
        indeChange(value) {
            this.formData.xingzhi = value ? options : []
            this.is = false
        }
    }
}
</script>

<style lang="scss" scoped>
.form {
    padding: 20px;
    max-width: 500px;
}
</style>

