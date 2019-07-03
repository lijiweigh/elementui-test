<template>
    <div class="table">
        <el-container v-loading="loading">
            <el-header class="header">
                <el-form :model="forms" inline >
                    <el-form-item  prop="name">
                        <el-input v-model="forms.name"  placeholder="姓名"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="search">查找</el-button>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="add">添加</el-button>
                    </el-form-item>
                </el-form>
            </el-header>
            <el-main>
                <el-table :data="users" style="width:100%" @current-change="handleChange" @selection-change="selectChange" highlight-current-row>
                    <el-table-column type="selection" width="55"></el-table-column>
                    <el-table-column type="index" width="60"></el-table-column>
                    <el-table-column label="姓名" prop="name" width="120" sortable></el-table-column>
                    <el-table-column label="性别" prop="sex" width="100" sortable></el-table-column>
                    <el-table-column label="年龄" prop="age" width="100" sortable></el-table-column>
                    <el-table-column label="生日" prop="birth" width="120" sortable></el-table-column>
                    <el-table-column label="地址" prop="address"  sortable></el-table-column>
                    <el-table-column label="操作" width="150">
                        <template #default="scope">
                            <el-button type="primary" @click="edit(scope.row, scope.$index)" size="small">编辑</el-button>
                            <el-button type="danger" @click="remove(scope.row,scope.$index)" size="small">删除</el-button>
                        </template>
                    </el-table-column>
                </el-table>
            </el-main>
            <el-footer>
                <el-row>
                    <el-col :span="4">
                        <el-button :disabled="disabled" type="danger" @click="mulRemove">批量删除</el-button>
                    </el-col>
                    <el-col :span="6" :offset="14">
                        <el-pagination layout="prev,pager,next" :current-page.sync="curPage" :total="50" :page-size="10" background @current-change="clickPage" @next-click="nextClick" @pre-click="preClick"></el-pagination>
                    </el-col>
                </el-row>
            </el-footer>
        </el-container>
        <el-dialog :before-close="beforeAddClose" :visible.sync="addShow" title="添加" :close-on-click-modal="false" :close-on-press-escape="false" center>
            <el-form ref="addForm" :model="addForm" :rules="addRules" label-position="left" label-width="auto">
                <el-form-item label="姓名" prop="name">
                    <el-input v-model="addForm.name"></el-input>
                </el-form-item>
                <el-form-item label="性别" prop="sex">
                    <el-radio-group v-model="addForm.sex">
                        <el-radio label="男"></el-radio>
                        <el-radio label="女"></el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="年龄" prop="age">
                    <el-input-number v-model="addForm.age" :min="18" :max="28"></el-input-number>
                </el-form-item>
                <el-form-item label="生日" prop="birth">
                    <el-date-picker v-model="addForm.birth" value-format="yyyy-MM-dd" format="yyyy-MM-dd"></el-date-picker>
                </el-form-item>
                <el-form-item label="地址" prop="address">
                    <el-input type="textarea" autosize v-model="addForm.address"></el-input>
                </el-form-item>
            </el-form>
            <template #footer>
                <el-button @click="cancelAdd">取消</el-button>
                <el-button type="success" @click="submitAdd">确定</el-button>
            </template>
        </el-dialog>
        <el-dialog :visible.sync="editForm" title="编辑" center :close-on-click-modal="false" :close-on-press-escape="false">
            <el-form :model="editData" label-position="left" label-width="auto">
                <el-form-item label="姓名">
                    <el-input v-model="editData.name"></el-input>
                </el-form-item>
                <el-form-item label="性别">
                    <el-radio-group v-model="editData.sex">
                        <el-radio label="男"></el-radio>
                        <el-radio label="女"></el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="年龄">
                    <el-input-number v-model="editData.age" :min="18" :max="28"></el-input-number>
                </el-form-item>
                <el-form-item label="生日">
                    <el-date-picker v-model="editData.birth" value-format="yyyy-MM-dd" format="yyyy-MM-dd"></el-date-picker>
                </el-form-item>
                <el-form-item label="地址">
                    <el-input v-model="editData.address" type="textarea" autosize></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button @click="cancelEdit">取消</el-button>
                    <el-button @click="submitEdit">确认</el-button>
                </el-form-item>
            </el-form>
        </el-dialog>
    </div>
</template>

<script>
import { setTimeout } from 'timers';

function createData () {
    let data = []
    for (let i = 0; i < 10; i++) {
        data.push({
            id: new Date().getTime(),
            name: String.fromCharCode(parseInt(Math.random() * 58 + 65)),
            sex: Math.random() > 0.5 ? "男" : "女",
            age: parseInt(Math.random() * 10 + 18),
            birth: `${new Date().getFullYear()}-${new Date().getMonth() + 1}-${new Date().getDay()}`,
            address: "北京 故宫"
        })
    }
    return data
}

export default {
    name: "mytable",
    data () {
        return {
            forms: {name: ""},
            inputRule: {
                name: [{
                    required: true, message: "姓名不能为空", trigger: "blur"
                }]
            },
            users: [],
            totalUsers: [],
            curPage: 1,
            selectedItems: [],
            loading: true,
            addShow: false,
            addForm: {name:"",sex: "",age: 18,birth: "",address:""},
            addRules: {
                name: [{required: true, message: "不能为空",trigger: "blur"}],
                sex: [{required: true, message: "不能为空",trigger: "change"}],
                // age: [{required: true, message: "不能为空",trigger: "blur"}],
                birth: [{required: true, message: "不能为空",trigger: "blur"}],
                address: [{required: true, message: "不能为空",trigger: "blur"}],
            },
            editForm: false,
            editData: {},
            editIndex: ""
        }
    },
    computed: {
        disabled () {
            return this.selectedItems.length === 0
        }
    },
    mounted () {
        setTimeout(() => {
            this.loading = false
            this.users = createData()
            this.totalUsers = [...this.users]
            console.log(this.users === this.totalUsers)
        }, 1000)
    },
    methods: {
        search () {
            this.loading = true
            
            let name = this.forms.name
            if (!name) {
                this.users = this.totalUsers
                
            } else {
                this.users = this.users.filter(item => {
                    return item.name === name
                })
            }
            
            setTimeout(() => {
                this.loading = false
            }, 500);
        },
        add () {
            this.addShow = true
        },
        beforeAddClose (done) {
            console.log("beforeAddClose")
            // this.$refs['addForm'].resetFields()
            done()
        },
        cancelAdd () {
            this.addShow = false
        },
        submitAdd () {
            this.$refs.addForm.validate(ok => {
                if (ok) {
                    this.users.push({...this.addForm})
                    this.totalUsers.push({...this.addForm})
                    this.addShow = false
                    console.log(this.addForm)
                    this.$nextTick(() => {
                        this.$refs['addForm'].resetFields()
                    })
                } else {
                    this.$alert("有内容未输入，请输入！", "验证不通过", {
                        type: "error"
                    })
                }
            })
            
        },
        edit (row,index) {
            console.log(row)
            this.editForm = true
            this.editData = {...row}
            this.editIndex = index
        },
        cancelEdit() {
            this.editForm = false
        },
        submitEdit() {
            this.$set(this.users, this.editIndex, this.editData)
            this.$set(this.totalUsers, this.editIndex, this.editData)
            this.editForm = false

        },
        remove (row,index) {
            console.log(row,index)
            this.$confirm("确认删除吗", "请确认", {type: "warning"}).then(() => {
                this.users.splice(index, 1)
                this.totalUsers.splice(index, 1)
            })
        },
        handleChange (row, oldrow) {
            console.log(row, oldrow)
        },
        selectChange (selection) {
            console.log(selection)
            this.selectedItems = selection
        },
        clickPage (page) {
            console.log(this.curPage)
        },
        nextClick (page) {
            console.log(this.curPage)
        },
        preClick (page) {
            console.log(this.curPage)
        },
        mulRemove () {
            this.users = this.users.filter(item => {
                return this.selectedItems.findIndex(i => {
                    return i === item
                }) < 0
            })
        }
    }
}
</script>

<style lang="scss" scoped>
.header {
    margin: 20px 0;
    padding: 10px;
    background: #eee;
}
</style>
