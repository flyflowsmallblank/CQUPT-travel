<template>
	<div>
		<el-row :gutter="20">
			<el-col :span="23">
				<el-row :gutter="20" class="mgb20">
					<el-col :span="8">
						<el-card shadow="hover" :body-style="{padding: '0px'}">
							<div class="grid-content grid-con-1">
								<el-avatar class="icon" src="https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png"></el-avatar>
								<div class="grid-cont-right">
									<div class="grid-num">{{ name }}</div>
									<div>管理员账号</div>
								</div>
							</div>
						</el-card>
					</el-col>
					<el-col :span="8">
						<el-card shadow="hover" :body-style="{padding: '0px'}">
							<div class="grid-content grid-con-2">
								<i class="el-icon-lx-notice grid-con-icon"></i>
								<div class="grid-cont-right">
									<div class="grid-num">{{ annoumcenentCount }}</div>
									<div>发布公告</div>
								</div>
							</div>
						</el-card>
					</el-col>
					<el-col :span="8">
						<el-card shadow="hover" :body-style="{padding: '0px'}">
							<div class="grid-content grid-con-3">
								<i class="el-icon-lx-goods grid-con-icon"></i>
								<div class="grid-cont-right">
									<div class="grid-num">{{ userCount }}</div>
									<div>注册用户</div>
								</div>
							</div>
						</el-card>
					</el-col>
				</el-row>
				<el-card shadow="hover" style="height:403px;">
					<div slot="header" class="clearfix">
						<span>待办事项</span>
						<el-button style="float: right; padding: 3px 0" type="text" @click="addTodo">添加</el-button>
					</div>
					<el-table :show-header="false" :data="todoList" style="width:100%;">
						<el-table-column width="40">
							<template slot-scope="scope">
								<el-checkbox v-model="scope.row.status" @change="toggleTodoStatus(scope.$index)"></el-checkbox>
							</template>
						</el-table-column>
						<el-table-column>
							<template slot-scope="scope">
								<div
									class="todo-item"
									:class="{'todo-item-del': scope.row.status}"
								>{{scope.row.title}}</div>
							</template>
						</el-table-column>
						<el-table-column width="60">
							<template slot-scope="scope">
								<i class="el-icon-edit" @click="editTodo(scope.$index, scope.row)"></i>
								<i class="el-icon-delete" @click="deleteTodo(scope.$index)"></i>
							</template>
						</el-table-column>
					</el-table>
				</el-card>
			</el-col>
		</el-row>
	</div>
</template>

<script>
import axios from 'axios'
import qs from 'qs'
import { Message, MessageBox } from 'element-ui'

export default {
	name: 'dashboard',
	data() {
		return {
			name: localStorage.getItem('userName'),
			annoumcenentCount: 0,
			userCount: 0,
			todoList: [
				{
					title: '今天要修复100个bug',
					status: false
				},
				{
					title: '今天要修复100个bug',
					status: false
				},
				{
					title: '今天要写100行代码加几个bug吧',
					status: false
				},
				{
					title: '今天要修复100个bug',
					status: false
				},
				{
					title: '今天要修复100个bug',
					status: true
				},
				{
					title: '今天要写100行代码加几个bug吧',
					status: true
				}
			],
		};
	},
	computed: {
		role() {
			return this.name = 'admin';
		}
	},
	created() {
		if (window.localStorage.getItem('userType') != 'admin') {
			Message.error("你没有管理员权限访问后台，即将跳转到登录页面")
			this.$router.push('/login')
		}
		else {
			let _this = this
			axios({
				method: 'get',
				url: 'http://localhost:8080/getUserList',
				headers: {
					'Content-type': 'application/x-www-form-urlencoded'
				}
			}).then(function (response) {
				_this.userCount = response.data.data.userList.length
			}).catch(function (error) {
				console.log(error)
			})

			axios({
				method: 'get',
				url: 'http://localhost:8080/getAnnouncementList',
				headers: {
					'Content-type': 'application/x-www-form-urlencoded'
				}
			}).then(function (response) {
				_this.annoumcenentCount = response.data.data.length
			}).catch(function (error) {
				console.log(error)
			})
		}
	},
	methods: {
		addTodo() {
			MessageBox.prompt('请输入待办事项', '添加待办事项', {
				confirmButtonText: '确定',
				cancelButtonText: '取消',
			}).then(({ value }) => {
				this.todoList.push({ title: value, status: false });
				Message.success('添加成功');
			}).catch(() => {
				Message.info('取消添加');
			});
		},
		editTodo(index, todo) {
			MessageBox.prompt('编辑待办事项', {
				confirmButtonText: '确定',
				cancelButtonText: '取消',
				inputValue: todo.title
			}).then(({ value }) => {
				this.$set(this.todoList, index, { ...todo, title: value });
				Message.success('编辑成功');
			}).catch(() => {
				Message.info('取消编辑');
			});
		},
		deleteTodo(index) {
			this.todoList.splice(index, 1);
			Message.success('删除成功');
		},
		toggleTodoStatus(index) {
			this.todoList[index].status = !this.todoList[index].status;
			Message.success('状态已更新');
		}
	}
};
</script>

<style scoped>
.icon {
	width: 60px;
	height: 60px;
	margin: 10px;
	margin-left: 30px;
}

.el-row {
	margin-bottom: 20px;
}

.grid-content {
	display: flex;
	align-items: center;
	height: 100px;
}

.grid-cont-right {
	flex: 1;
	text-align: center;
	font-size: 14px;
	color: #999;
}

.grid-num {
	font-size: 30px;
	font-weight: bold;
}

.grid-con-icon {
	font-size: 50px;
	width: 100px;
	height: 100px;
	text-align: center;
	line-height: 100px;
	color: #fff;
}

.grid-con-1 .grid-con-icon {
	background: rgb(45, 140, 240);
}

.grid-con-1 .grid-num {
	color: rgb(45, 140, 240);
}

.grid-con-2 .grid-con-icon {
	background: rgb(100, 213, 114);
}

.grid-con-2 .grid-num {
	color: rgb(45, 140, 240);
}

.grid-con-3 .grid-con-icon {
	background: rgb(242, 94, 67);
}

.grid-con-3 .grid-num {
	color: rgb(242, 94, 67);
}

.user-info {
	display: flex;
	align-items: center;
	padding-bottom: 20px;
	margin-bottom: 20px;
}

.user-avator {
	width: 120px;
	height: 120px;
	border-radius: 50%;
}

.user-info-cont {
	padding-left: 50px;
	flex: 1;
	font-size: 14px;
	color: #999;
}

.user-info-cont div:first-child {
	font-size: 30px;
	color: #222;
}

.user-info-list {
	font-size: 14px;
	color: #999;
	line-height: 25px;
}

.user-info-list span {
	margin-left: 70px;
}

.mgb20 {
	margin-bottom: 20px;
}

.todo-item {
	font-size: 14px;
}

.todo-item-del {
	text-decoration: line-through;
	color: #999;
}

</style>
