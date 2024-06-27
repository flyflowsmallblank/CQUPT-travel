<template>
	<div class="personal-card">
		<el-row :gutter="24" justify="center">
			<el-col :span="16" style="width: 100%">
				<el-tabs type="card" v-model="activePanel">
					<el-tab-pane class="panel" label="用户信息" name="first">
						<el-form label-width="80px" style="width: 50%; margin: 0 auto;">
							<el-form-item label="用户名">
								<el-input v-model="userInfo.UserName" :disabled="editable"></el-input>
							</el-form-item>
							<el-form-item label="密码">
								<el-input v-model="userInfo.UserPassword" :disabled="editable"></el-input>
							</el-form-item>
							<el-form-item label="联系方式">
								<el-input v-model="userInfo.UserPhone" :disabled="editable"></el-input>
							</el-form-item>
							<el-form-item label="性别">
								<el-input v-model="userInfo.UserSex" :disabled="editable"></el-input>
							</el-form-item>
							<el-form-item label="年龄">
								<el-input v-model="userInfo.UserAge" :disabled="editable"></el-input>
							</el-form-item>
							<el-form-item label="个人介绍">
								<el-input type="textarea" :autosize="{ minRows: 2, maxRows: 8}" v-model="userInfo.UserDesc" :disabled="editable"></el-input>
							</el-form-item>
							<el-form-item label="修改头像" v-if="!editable">
								<el-upload
									class="avatar-uploader"
									action="http://localhost:8080/uploadUserProfile"
									list-type="picture-card"
									:show-file-list="true"
									:on-preview="handlePictureCardPreview"
									:on-remove="handleRemove"
									:auto-upload="false"
									ref="upload"
									:on-success="onSuccess"
									:on-progress="onProgress"
									:limit="1"
								>
									<i class="el-icon-plus"></i>
								</el-upload>
								<el-dialog :visible.sync="dialogVisible">
									<img width="100%" :src="imgURL" alt="">
								</el-dialog>
							</el-form-item>
						</el-form>
						<el-row class="manageUser-toolbar" style="text-align: center;">
							<el-button v-if="editable" type="primary" icon="el-icon-edit" @click="onModify()">修改信息</el-button>
							<el-button v-else type="primary" icon="el-icon-arrow-left" @click="onCancel()">放弃修改</el-button>
							<el-button type="success" icon="el-icon-check" @click="onSubmitEdit()" :disabled="editable">提交修改</el-button>
						</el-row>
					</el-tab-pane>
					<el-tab-pane class="panel" label="账号管理" name="fourth">
						<el-button type="danger" icon="el-icon-s-promotion" @click="quit()">退出账号</el-button>
					</el-tab-pane>
				</el-tabs>
			</el-col>
		</el-row>
	</div>
</template>

<script>
import axios from 'axios';
import qs from 'qs';
import { Message } from 'element-ui';

export default {
	name: 'personal_page',
	data() {
		return {
			activePanel: 'first',
			editable: true,
			userName: '',
			userInfo: {},
			dialogVisible: false,
			imgURL: ''
		};
	},
	methods: {
		quit() {
			window.localStorage.setItem('userName', '');
			window.localStorage.setItem('userType', '');
			window.localStorage.setItem('userProfile', '');
			this.$router.push('/login');
			Message.success('退出当前账号');
		},
		onModify() {
			this.editable = false;
		},
		onProgress() {
			// Handle progress if needed
		},
		handleRemove(file, fileList) {
			console.log(file, fileList);
		},
		handlePictureCardPreview(file) {
			this.imgURL = file.url;
		},
		onSubmitEdit() {
			this.$refs.upload.submit();
		},
		onSuccess: function (response){
			if (response.status === 'success') {
				this.userInfo.userProfile = response.imgURL;
				let _this = this;
				axios({
					method: 'post',
					url: 'http://localhost:8080/updateUser',
					headers: {
						'Content-type': 'application/x-www-form-urlencoded'
					},
					data: qs.stringify({
						userID: this.userInfo.userID,
						userNamePrev: this.userName,
						userName: this.userInfo.userName,
						userPassword: this.userInfo.userPassword,
						userSex: this.userInfo.userSex,
						userAge: this.userInfo.userAge,
						userPhone: this.userInfo.userPhone,
						userDesc: this.userInfo.userDesc,
						userStatus: this.userInfo.userStatus,
						userProfile: this.userInfo.userProfile
					})
				}).then(function (response) {
					let status = response.data.status;
					if (status === 'failed') {
						Message.error('该用户名已经被别人占用啦，换一个吧~');
						_this.userName = window.localStorage.getItem('userName');
						_this.userInfo.userName = _this.userName;
					} else {
						Message.success('修改成功');
						_this.userInfo = response.data.data;
						_this.userName = _this.userInfo.userName;
						window.localStorage.setItem('userName', _this.userName);
						_this.editable = true;
					}
				}).catch(function (error) {
					console.log(error);
				});
			} else {
				Message.error('头像上传失败，请重试');
			}
		},
		onCancel() {
			this.editable = true;
		}
	},
	created() {
		if (window.localStorage.getItem('userType') !== 'user') {
			Message.error('请先登录，即将跳转到登录页面');
			this.$router.push('/login');
		} else {
			this.userName = window.localStorage.getItem('userName');
			let _this = this;
			axios({
				method: 'post',
				url: 'http://localhost:8080/getUser',
				headers: {
					'Content-type': 'application/x-www-form-urlencoded'
				},
				data: qs.stringify({
					userName: _this.userName,
				})
			}).then(function (response) {
				let status = response.data.status;
				if (status === 'failed') {
					Message.error('加载用户信息失败');
				} else {
					_this.userInfo = response.data.data;
					window.localStorage.setItem('userProfile', response.data.data.userProfile);
					console.log(_this.userInfo)
				}
			}).catch(function (error) {
				console.log(error);
			});
		}
	}
};
</script>

<style scoped>
.personal-card {
	padding-left: 50px;
	padding-right: 50px;
	margin-top: 30px;
	width: 100%;
}

.panel {
	padding-top: 20px;
}

.user-info-cont p {
	font-size: 12px;
	color: gray;
}
</style>
