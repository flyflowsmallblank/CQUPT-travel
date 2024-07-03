<template>
<div class='manageUser-main'>
    <el-row class="manageUser-toolbar">
        <el-button type="primary" icon="el-icon-plus" @click="addUser()">添加用户</el-button>
        <!-- <el-col :span="4" :offset="0"><el-tag style="margin-left:300px">用户信息管理 | 数据无价 小心操作</el-tag></el-col> -->
    </el-row>
    <el-row class="data-table" >
        <el-table :data="tableData.slice((currentPage - 1)* pagesize, currentPage * pagesize)" border style="width: 65%">
            <el-table-column label="用户ID" width="100px">
                <template slot-scope="scope">
                    <span>{{ scope.row.userID }}</span>
                </template>
            </el-table-column>
            <el-table-column label="用户名" width="150px">
                <template slot-scope="scope">
                    <el-popover trigger="hover" placement="top">
                        <div class="user-info">
                            <img :src=scope.row.userProfile class="user-avator" alt style="height: 100px;" />
                            <div class="user-info-cont">
                                <p>名称: {{ scope.row.userName }}</p>
                                <p>密码: *******</p>
                                <p>介绍: {{ scope.row.userDesc }}</p>
                            </div>
                        </div>
                        <div slot="reference" class="name-wrapper">
                            <span style="color:rgb(64,158,255);">{{ scope.row.userName }}</span>
                        </div>
                    </el-popover>
                </template>
            </el-table-column>
            <el-table-column label="密码" width="180px">
                <p>
					*******
				</p>
            </el-table-column>
            <el-table-column label="性别" width="100px">
                <template slot-scope="scope">
                    <span>{{ scope.row.userSex }}</span>
                </template>
            </el-table-column>
            <el-table-column label="年龄" width="100px">
                <template slot-scope="scope">
                    <span>{{ scope.row.userAge }}</span>
                </template>
            </el-table-column>
            <el-table-column label="电话" width="180px">
                <template slot-scope="scope">
                    <span>{{ scope.row.userPhone }}</span>
                </template>
            </el-table-column>
            <el-table-column label="状态" width="100px;">
                <template slot-scope="scope">
                    <el-tag type="success" style="margin-top: -5px;" size="small">{{ scope.row.userStatus }}</el-tag>
                </template>
            </el-table-column>
            <el-table-column label="操作" :span="1" width="140px;">
                <template slot-scope="scope">
                    <el-button size="mini" type="primary" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                    <el-popconfirm confirm-button-text='删除' cancel-button-text='取消' @confirm="handleDelete(scope.$index, scope.row)" icon="el-icon-info" icon-color="red" title="确定删除该用户吗？">
                        <el-button slot="reference" size="mini" type="danger"  style="margin-left: -60px;">删除</el-button>
                    </el-popconfirm>
                </template>
            </el-table-column>
        </el-table>
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage" :page-sizes="[5, 10]" :page-size="pagesize" background layout="total, sizes, prev, pager, next, jumper" :total="tableData.length" style="padding-top:20px">
        </el-pagination>
    </el-row>
    <el-dialog :title="dialogTitle" :visible.sync="dialogVisible" width="30%">
        <el-form ref="form" :model="form" :rules='rules' label-width="80px">
            <el-alert v-if="!editable" title="管理员添加用户时只能使用默认头像" type="warning" style="margin-bottom: 20px; margin-top:-20px;"></el-alert>
            <el-form-item label="姓名" prop="userName">
                <el-input v-model="form.userName"></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="userPassword">
                <el-input v-model="form.userPassword" show-password></el-input>
            </el-form-item>
            <el-form-item label="年龄" prop="userAge">
                <el-input v-model="form.userAge"></el-input>
            </el-form-item>
			<el-form-item label="性别" prop="userSex">
				<el-select v-model="form.userSex" placeholder="请选择性别" style="left: -129px">
					<el-option label="男" value="男"></el-option>
					<el-option label="女" value="女"></el-option>
				</el-select>
			</el-form-item>
            <el-form-item label="电话" prop="userSex">
                <el-input v-model="form.userPhone"></el-input>
            </el-form-item>
            <el-form-item label="介绍" prop="userDesc">
                <el-input type="textarea" :autosize="{ minRows: 2, maxRows: 8}" placeholder="请输入内容" v-model="form.userDesc"></el-input>
            </el-form-item>
			<el-form-item label="权限" prop="userStatus">
				<el-input v-model="form.userStatus"></el-input>
			</el-form-item>
        </el-form>
        <span slot="footer" class="dialog-footer">
            <el-button @click="dialogVisible = false">取 消</el-button>
            <el-button v-if="dialogTitle === '添加用户'" type="primary" @click="handleAddUser('form')">添 加</el-button>
            <el-button v-else type="primary" @click="handleModifyUser('form')">修 改</el-button>
        </span>
    </el-dialog>
</div>
</template>

<style scoped>
.data-table {
    margin-top: 20px;
	display:flex;
	justify-content: center;
	flex-direction: column;
	align-items: center;
}
</style>

<script>
const axios = require('axios')
const qs = require('qs')
const ele = require('element-ui')

export default {
    name: 'manageUser',
    created() {
		this.getUserList()
    },
    data() {
        return {
            dialogTitle: '添加用户',
            currentUserID: '',
            prevUserName: '',
            editable: false,
            currentPage: 1,
            pagesize: 10,
            dialogVisible: false,
            form: {
                userName: '',
                userPassword: '',
                userAge: '',
                userSex: '',
                userPhone: '',
                userDesc: '',
                userStatus: ''
            },
            tableData: [],
            // 表单验证，需要在 el-form-item 元素中增加 prop 属性
            rules: {
                userName: [{
                    required: true,
                    message: '用户名为必填项',
                    trigger: 'blur'
                }],
                userPassword: [{
                        required: true,
                        message: '密码为必填项',
                        trigger: 'blur'
                    },
                    {
                        min: 6,
                        max: 30,
                        message: "密码长度在6-30之间",
                        trigger: 'blur'
                    }
                ],
				userPhone:[{
					required: true,
					message: "手机号码为必填项",
					trigger: "blur",
				},
					{
						pattern: /^1[3456789]\d{9}$/,
						message: "手机号码格式不正确",
						trigger: "blur",
					}]
            },
        }
    },
    methods: {
		getUserList(){
			if (window.localStorage.getItem('userType') != 'admin') {
				ele.Message.error("你没有管理员权限访问后台，即将跳转到登录页面")
				this.$router.push('/login')
			} else {
				let _this = this
				axios({
					method: 'get',
					url: 'http://115.159.4.245:8080/getUserList',
					headers: {
						'Content-type': 'application/x-www-form-urlencoded'
					}
				}).then(function (response) {
					ele.Message.success('加载用户数据成功')
					_this.tableData = response.data.data.userList
				}).catch(function (error) {
					console.log(error)
				})
			}
		},
		handleModifyUser(formName) {
			this.$refs[formName].validate((valid) => {
				if (valid) {
					axios.post('http://115.159.4.245:8080/updateUserAdmin', qs.stringify({
						userID: this.currentUserID,
						userNamePrev: this.prevUserName,
						userName: this.form.userName,
						userPassword: this.form.userPassword,
						userSex: this.form.userSex,
						userAge: this.form.userAge,
						userPhone: this.form.userPhone,
						userDesc: this.form.userDesc,
						userProfile: this.form.userProfile,
						userStatus: this.form.userStatus
					}), {
						headers: { 'Content-type': 'application/x-www-form-urlencoded' }
					}).then(response => {
						if (response.data.code !== 0) {
							ele.Message.error("该用户名已经被别人占用啦，换一个吧~");
						} else {
							ele.Message.success('更新用户数据成功');
							_this.getUserList()
							_this.dialogVisible = false
						}
					}).catch(error => {
						console.log(error);
					});
				} else {
					this.dialogVisible = true;
					return false;
				}
			});
		},
        handleEdit(index, row) {
            this.dialogTitle = "修改用户信息"
            this.dialogVisible = true
            this.editable = true

            this.prevUserName = row.userName
            this.currentUserID = row.userID
            this.form.userName = row.userName
            this.form.userPassword = row.userPassword
            this.form.userAge = row.userAge
            this.form.userProfile = row.userProfile
            this.form.userSex = row.userSex
            this.form.userPhone = row.userPhone
            this.form.userDesc = row.userDesc
        },
        handleDelete(index, row) {
            let _this = this
            axios({
                method: 'post',
                url: 'http://115.159.4.245:8080/deleteUser',
                headers: {
                    'Content-type': 'application/x-www-form-urlencoded'
                },
                data: qs.stringify({
                    userID: row.userID
                })
            }).then(function (response) {
                ele.Message.success('更新用户数据成功')
				_this.getUserList()
				_this.dialogVisible = false
			}).catch(function (error) {
                console.log(error)
            })
        },
        addUser() {
            this.dialogTitle = "添加用户"
            this.dialogVisible = true
            this.editable = false

            this.prevUserName = ''
            this.currentUserID = ''
            this.form.userName = ''
            this.form.userPassword = ''
            this.form.userAge = ''
            this.form.userSex = ''
            this.form.userPhone = ''
            this.form.userDesc = ''
            this.form.userStatus = ''
            this.form.userProfile = ''
        },
        handleAddUser(formName) {
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    let _this = this
                    axios({
                        method: 'post',
                        url: 'http://115.159.4.245:8080/addUser',
                        headers: {
                            'Content-type': 'application/x-www-form-urlencoded'
                        },
                        data: qs.stringify({
                            userName: this.form.userName,
                            userPassword: this.form.userPassword,
                            userSex: this.form.userSex,
                            userAge: this.form.userAge,
                            userPhone: this.form.userPhone,
                            userDesc: this.form.userDesc
                        })
                    }).then(function (response) {
                        let code = response.data.code
						console.log(code)
                        if (code !== 0) {
                            ele.Message.error("该用户名已经被别人占用啦，换一个吧~")
                        } else {
                            ele.Message.success('新建用户成功')
                            _this.tableData = response.data.data.userList
							_this.getUserList()
							_this.dialogVisible = false
						}
                    }).catch(function (error) {
                        console.log(error)
                    })
                } else {
                    this.dialogVisible = true
                    return false
                }
            })
        },
        // 初始页currentPage、初始每页数据数pagesize和数据data
        handleSizeChange: function (size) {
            this.pagesize = size;
            console.log(this.pagesize) //每页下拉显示数据
        },
        handleCurrentChange: function (currentPage) {
            this.currentPage = currentPage;
            console.log(this.currentPage) //点击第几页
        }
    }
}
</script>
