<template>
<div>
    <div class="top-bar">
        <el-button type="primary" icon="el-icon-edit" @click="addDormitory()">添加宿舍</el-button>
    </div>
    <el-col :span="23" class="data-table">
        <el-table :data="tableData.slice((currentPage - 1)* pagesize, currentPage * pagesize)" border style="width: 100%">
            <el-table-column label="ID" width="80px" fixed>
                <template slot-scope="scope">
                    <span>{{ scope.row.dormitoryID }}</span>
                </template>
            </el-table-column>
            <el-table-column label="宿舍名称" width="150px" fixed>
                <template slot-scope="scope">
                    <el-popover trigger="hover" placement="top">
                        <p><i style="color:rgb(64,158,255);" class="el-icon-info">宿舍名称</i>&nbsp;&nbsp;{{ scope.row.dormitoryName }}</p>
                        <p><i style="color:rgb(64,158,255);" class="el-icon-s-promotion">发布时间</i>&nbsp;&nbsp;{{ scope.row.createdAt }}</p>
                        <p><i style="color:rgb(64,158,255);" class="el-icon-s-ticket">宿舍价格</i>&nbsp;&nbsp;{{ scope.row.dormitoryPrice }}</p>
                        <p><i style="color:rgb(64,158,255);" class="el-icon-s-comment">宿舍介绍</i>&nbsp;&nbsp;{{ scope.row.dormitoryDesc }}</p>
                        <img :src="scope.row.dormitoryPicture" style="height:300px" />
                        <div slot="reference" class="name-wrapper">
                            <span style="color:rgb(64,158,255);">{{ scope.row.dormitoryName }}</span>
                        </div>
                    </el-popover>
                </template>
            </el-table-column>
            <el-table-column label="宿舍地址" width="200px" sortable>
                <template slot-scope="scope">
                    <span style="margin-left: 10px">{{ scope.row.dormitoryAddress }}</span>
                </template>
            </el-table-column>
            <el-table-column label="宿舍价格" width="140px" sortable>
                <template slot-scope="scope">
                    <span style="margin-left: 10px">{{ scope.row.dormitoryPrice }}</span>
                </template>
            </el-table-column>
            <el-table-column label="联系电话" width="200px" sortable>
                <template slot-scope="scope">
                    <span style="margin-left: 10px">{{ scope.row.dormitoryPhone }}</span>
                </template>
            </el-table-column>
            <el-table-column label="宿舍星级" width="140px" sortable>
                <template slot-scope="scope">
                    <span style="margin-left: 10px">{{ scope.row.dormitoryStar }}</span>
                </template>
            </el-table-column>
            <el-table-column label="余量" :span="1" sortable>
                <template slot-scope="scope">
                    <span style="margin-left: 10px">{{ scope.row.dormitoryRemain }}</span>
                </template>
            </el-table-column>
            <el-table-column label="图片" width="380px">
                <template slot-scope="scope">
                    <span>{{ scope.row.dormitoryPicture.slice(0, 40) + '...'}}</span>
                </template>
            </el-table-column>
            <el-table-column label="发布者" width="150px" sortable>
                <template slot-scope="scope">
                    <span style="margin-left: 10px">{{ scope.row.dormitoryAuthor }}</span>
                </template>
            </el-table-column>
            <el-table-column label="发布时间" width="250px" sortable>
                <template slot-scope="scope">
                    <span style="margin-left: 10px">{{ scope.row.createdAt }}</span>
                </template>
            </el-table-column>
            <el-table-column label="操作" width="140px;" fixed="right">
                <template slot-scope="scope">
                    <el-button size="mini" type="primary" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                    <el-popconfirm confirm-button-text='删除' cancel-button-text='取消' @confirm="handleDelete(scope.$index, scope.row)" icon="el-icon-info" icon-color="red" title="确定删除吗？">
                        <el-button slot="reference" size="mini" type="danger" style="margin-left: -60px;">删除</el-button>
                    </el-popconfirm>
                </template>
            </el-table-column>
        </el-table>
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage" :page-sizes="[5, 10]" :page-size="pagesize" background layout="total, sizes, prev, pager, next, jumper" :total="tableData.length" style="padding-top:20px">
        </el-pagination>
    </el-col>
    <el-dialog :title="dialogTitle" :visible.sync="dialogVisible" width="40%">
        <el-form ref="form" :model="form" :rules='rules' label-width="80px">
            <el-alert v-if="editable" title="管理员无法修改图片内容" type="warning" style="margin-bottom: 20px; margin-top:-20px;"></el-alert>
            <el-form-item label="宿舍名称" prop="dormitoryName">
                <el-input v-model="form.dormitoryName"></el-input>
            </el-form-item>
            <el-form-item label="宿舍介绍" prop="dormitoryDesc">
                <el-input v-model="form.dormitoryDesc"></el-input>
            </el-form-item>
            <el-form-item label="宿舍地址" prop="dormitoryAddress">
                <el-input v-model="form.dormitoryAddress"></el-input>
            </el-form-item>
            <el-form-item label="宿舍价格" prop="dormitoryPrice">
                <el-input v-model="form.dormitoryPrice"></el-input>
            </el-form-item>
            <el-form-item label="宿舍星级" prop="dormitoryStar">
                <el-input v-model="form.dormitoryStar"></el-input>
            </el-form-item>
            <el-form-item label="联系电话" prop="dormitoryPhone">
                <el-input v-model="form.dormitoryPhone"></el-input>
            </el-form-item>
            <el-form-item label="余量" prop="dormitoryRemain">
                <el-input v-model="form.dormitoryRemain"></el-input>
            </el-form-item>
            <el-form-item v-show=!editable label="添加图片">
                <el-upload class="avatar-uploader" action="http://115.159.4.245:8080/uploadPicture" list-type="picture-card" :show-file-list="true" :on-preview="handlePictureCardPreview" :on-remove="handleRemove" :auto-upload="false" ref="upload" :on-success="onSuccess" :on-progress="onProgress" :limit="1">
                    <i class="el-icon-plus"></i>
                </el-upload>
                <el-dialog :visible.sync="imgDialogVisible">
                    <img width="100%" :src="imgURL" alt="">
                </el-dialog>
            </el-form-item>
            <el-form-item label="发布者">
                <el-input v-model=userName :disabled=true></el-input>
            </el-form-item>
        </el-form>
        <span slot="footer" class="dialog-footer">
            <el-button v-if="dialogTitle === '添加宿舍'" type="primary" @click="handleAddDormitory('form')">发 布</el-button>
            <el-button v-else type="primary" @click="handleModifyDormitory('form')">修 改</el-button>
        </span>
    </el-dialog>

</div>
</template>

<script>
const axios = require('axios')
const qs = require('qs')
const ele = require('element-ui')

export default {
    name: 'manageDormitory',
    data() {
        return {
            currentPage: 1,
            pagesize: 5,
            tableData: [],
            userName: '',
            dialogVisible: false,
            imgDialogVisible: false,

            // 修改公告时不能编辑图片
            editable: true,
            imgURL: '',
            currentDormitoryID: '',
            dialogTitle: '添加宿舍',
            form: {
                dormitoryName: '',
                dormitoryDesc: '',
                dormitoryAddress: '',
                dormitoryPrice: '',
                dormitoryStar: '',
                dormitoryPhone: '',
                dormitoryRemain: '',
                dormitoryPicture: '',
                dormitoryAuthor: ''
            },
            rules: {
                dormitoryName: [{
                    required: true,
                    message: '必填项',
                    trigger: 'blur'
                }],
                dormitoryDesc: [{
                    required: true,
                    message: '必填项',
                    trigger: 'blur'
                }],
                dormitoryAddress: [{
                    required: true,
                    message: '必填项',
                    trigger: 'blur'
                }],
                dormitoryPrice: [{
                    required: true,
                    message: '必填项',
                    trigger: 'blur'
                }],
                dormitoryStar: [{
                    required: true,
                    message: '必填项',
                    trigger: 'blur'
                }],
                dormitoryPhone: [{
                    required: true,
                    message: '必填项',
                    trigger: 'blur'
                }],
                dormitoryRemain: [{
                    required: true,
                    message: '必填项',
                    trigger: 'blur'
                }]
            },
        }
    },
    created() {
		this.getDormitoryList()
    },
    methods: {
		getDormitoryList() {
			let _this = this
			this.userName = window.localStorage.getItem('userName')
			axios({
				method: 'get',
				url: 'http://115.159.4.245:8080/getDormitoryList',
				headers: {
					'Content-type': 'application/x-www-form-urlencoded'
				}
			}).then(function (response) {
				if (Array.isArray(response.data.data)) {
					_this.tableData = response.data.data;
				} else {
					console.error('API 响应的数据结构不正确:', response.data);
				}
			}).catch(function (error) {
				console.log(error)
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
        },
        addDormitory() {
            this.dialogVisible = true
            this.dialogTitle = '添加宿舍'
            this.editable = false

            // 表格中的内容（this.form）应该要清空才对，为了测试方便暂时不清空
        },
        handleEdit(index, row) {
            this.dialogTitle = "修改宿舍信息"
            this.dialogVisible = true
            this.editable = true

            this.currentDormitoryID = row.dormitoryID,
                this.form.dormitoryName = row.dormitoryName,
                this.form.dormitoryDesc = row.dormitoryDesc,
                this.form.dormitoryAddress = row.dormitoryAddress,
                this.form.dormitoryPrice = row.dormitoryPrice,
                this.form.dormitoryStar = row.dormitoryStar,
                this.form.dormitoryPhone = row.dormitoryPhone,
                this.form.dormitoryRemain = row.dormitoryRemain,
                this.form.dormitoryPicture = row.dormitoryPicture
        },
        onProgress() {},
        handleRemove(file, fileList) {
            console.log(file, fileList);
        },
        handlePictureCardPreview(file) {
            this.imgURL = file.url
        },
        handleAddSite(formName) {
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    this.$refs.upload.submit();
                    // 后面执行onSuccess函数
                } else {
                    ele.Message.error("请按要求输入表格内容")
                }
            })
        },
        handleAddDormitory(formName) {
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    this.$refs.upload.submit();
                    // 后面执行onSuccess函数
                } else {
                    ele.Message.error("请按要求输入表格内容")
                }
            })
        },
        onSuccess: function (response) {
            if (response.status === "success") {
                this.form.dormitoryPicture = response.imgURL
                let _this = this
                axios({
                    method: 'post',
                    url: 'http://115.159.4.245:8080/addDormitory',
                    headers: {
                        'Content-type': 'application/x-www-form-urlencoded'
                    },
                    data: qs.stringify({
                        dormitoryName: this.form.dormitoryName,
                        dormitoryDesc: this.form.dormitoryDesc,
                        dormitoryAddress: this.form.dormitoryAddress,
                        dormitoryPrice: this.form.dormitoryPrice,
                        dormitoryStar: this.form.dormitoryStar,
                        dormitoryPhone: this.form.dormitoryPhone,
                        dormitoryRemain: this.form.dormitoryRemain,
                        dormitoryPicture: this.form.dormitoryPicture,
                        dormitoryAuthor: this.userName
                    })
                }).then(function (response) {
                    let status = response.data.data.status
                    if (status == 'success') {
                        ele.Message.success('创建宿舍成功')
                        _this.tableData = response.data.data
						_this.getDormitoryList()
                        _this.dialogVisible = false
                    } else {
                        ele.Message.error("创建宿舍失败")
                    }
                }).catch(function (error) {
                    console.log(error)
                })
            } else {
                ele.Message.error("图像上传失败，请重试")
            }
        },
        handleModifyDormitory(formName) {
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    let _this = this
                    axios({
                        method: 'post',
                        url: 'http://115.159.4.245:8080/updateDormitory',
                        headers: {
                            'Content-type': 'application/x-www-form-urlencoded'
                        },
                        data: qs.stringify({
                            dormitoryID: this.currentDormitoryID,
                            dormitoryName: this.form.dormitoryName,
                            dormitoryDesc: this.form.dormitoryDesc,
                            dormitoryAddress: this.form.dormitoryAddress,
                            dormitoryPrice: this.form.dormitoryPrice,
                            dormitoryStar: this.form.dormitoryStar,
                            dormitoryPhone: this.form.dormitoryPhone,
                            dormitoryRemain: this.form.dormitoryRemain,
                            dormitoryPicture: this.form.dormitoryPicture,
                            dormitoryAuthor: this.userName
                        })
                    }).then(function (response) {
                        ele.Message.success('更新宿舍数据成功')
                        _this.tableData = response.data.data
						_this.getDormitoryList()
						_this.dialogVisible = false
                    }).catch(function (error) {
                        console.log(error)
                    })
                } else {
                    this.dialogVisible = true
                    return false
                }
            })
        },
        handleDelete(index, row) {
            let _this = this
            axios({
                method: 'post',
                url: 'http://115.159.4.245:8080/deleteDormitory',
                headers: {
                    'Content-type': 'application/x-www-form-urlencoded'
                },
                data: qs.stringify({
                    dormitoryID: row.dormitoryID
                })
            }).then(function (response) {
                ele.Message.success('更新数据成功')
                _this.tableData = response.data.data
				_this.getDormitoryList()
				_this.dialogVisible = false
            }).catch(function (error) {
                console.log(error)
            })
        }
    }
}
</script>

<style scoped>
.data-table {
    margin-top: 20px;
}
</style>
