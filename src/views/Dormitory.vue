<template>
	<div class="dormitory-main">
		<el-container>
			<!-- 左列 -->
			<el-main class="dormitory-left" style="width: 50%; overflow-y: auto;">
				<el-table :data="tableData" border style="width: 100%" class="dormitory-card" :show-header="false">
					<el-table-column width="80px" class="dormitory-card-column">
						<template slot-scope="scope">
							<el-avatar :size="40" style="background: rgb(250, 172, 98);" v-if='scope.$index == 0'>
								<p style="margin-top: 0px;">{{ scope.$index + 1 }}</p>
							</el-avatar>
							<el-avatar :size="40" style="background: rgb(94, 153, 183);" v-else-if='scope.$index == 1'>
								<p style="margin-top: 0px;">{{ scope.$index + 1 }}</p>
							</el-avatar>
							<el-avatar :size="40" style="background: rgb(83, 141, 153);" v-else-if='scope.$index == 2'>
								<p style="margin-top: 0px;">{{ scope.$index + 1 }}</p>
							</el-avatar>
							<el-avatar :size="40" style="background: rgb(148, 166, 214);" v-else>
								<p style="margin-top: 0px;">{{ scope.$index + 1 }}</p>
							</el-avatar>
						</template>
					</el-table-column>
					<el-table-column :span="1">
						<template slot-scope="scope">
							<el-row>
								<el-col :span="8">
									<div @click="detail(scope.$index)">
										<img class="desc-img" :src="scope.row.dormitoryPicture"/>
									</div>
								</el-col>
								<el-col :span="16">
									<div class="grid-cont-right" @click="detail(scope.$index)">
										<h2>{{ scope.row.dormitoryName }}</h2>
										<p>
											<i style="color:#42b983;" class="el-icon-s-promotion">发布时间</i>&nbsp;&nbsp;{{
												scope.row.dormitoryDate
											}}
										</p>
										<p>
											<i style="color:#42b983;" class="el-icon-location">宿舍地址</i>&nbsp;&nbsp;{{
												scope.row.dormitoryAddress
											}}
										</p>
									</div>
								</el-col>
							</el-row>
						</template>
					</el-table-column>
				</el-table>
				<el-footer>到底了</el-footer>
			</el-main>

			<!-- 右列 -->
			<el-aside class="dormitory-right" style="width: 50%; overflow-y: auto;">
				<el-card class="box-card">
					<div slot="header" class="clearfix">
						<span>宿舍详情</span>
					</div>
					<div style="text-align:left;">
						<h2>{{ dormitoryName }}</h2>
						<el-rate v-model="dormitoryStar" disabled show-score text-color="#ff9900"></el-rate>
						<p>
							<i style="color:#42b983;"
							   class="el-icon-s-promotion">发布时间</i>&nbsp;&nbsp;&nbsp;{{ dormitoryDate }}
						</p>
						<p>
							<i style="color:#42b983;"
							   class="el-icon-location">宿舍地址</i>&nbsp;&nbsp;&nbsp;{{ dormitoryAddress }}
						</p>
						<p>
							<i style="color:#42b983;"
							   class="el-icon-s-ticket">宿舍介绍</i>&nbsp;&nbsp;&nbsp;{{ dormitoryDesc }}
						</p>
						<img :src="dormitoryPicture" style="width:80%;"/>
					</div>
				</el-card>

				<div class="comments-section">
					<el-card class="comment-card">
						<div class="comment-form">
							<p class="comment_place_text">评论区</p>
							<el-input class="comment_input_place" v-model="newComment" type="textarea"
									  placeholder="留下你的精彩想法吧"
									  :autosize="{ minRows: 5, maxRows: 10 }"></el-input>
							<el-button type="primary" @click="addComment">提交</el-button>
						</div>
					</el-card>

					<el-card class="comment-card" v-for="comment in comments" :key="comment.id">
						<div class="comment-header">
							<el-avatar :size="40" :src="require('@/assets/user.svg')"></el-avatar>
							<div class="comment-info">
								<h3>{{ comment.username }}</h3>
								<p class="comment-time">{{ comment.time }}</p>
							</div>
						</div>
						<div class="comment-body">
							<p>{{ comment.content }}</p>
						</div>
					</el-card>
				</div>
			</el-aside>
		</el-container>
	</div>
</template>

<script>
import qs from "qs";
import axios from "axios";
import ele from "element-ui";

export default {
	name: 'dormitory_page',
	data() {
		return {
			curUser: {
				userID: '',
				userName: ''
			},
			dormitoryID: '',
			dormitoryName: '',
			dormitoryDate: '',
			dormitoryAddress: '',
			dormitoryPrice: '',
			dormitoryDesc: '',
			dormitoryAuthor: '',
			dormitoryStar: 0,
			dormitoryPicture: '',
			tableData: [],
			comments: [
				// {
				// 	id: 1,
				// 	username: 'Alice Johnson',
				// 	avatarUrl: 'https://via.placeholder.com/40',
				// 	time: '2024-06-15 14:23',
				// 	content: 'This is a great article! I found it very helpful.',
				// },
				// {
				// 	id: 2,
				// 	username: 'Bob Brown',
				// 	avatarUrl: 'https://via.placeholder.com/40',
				// 	time: '2024-06-14 09:12',
				// 	content: 'I have some questions regarding the topic.',
				// },
				// {
				// 	id: 3,
				// 	username: 'Carol Green',
				// 	avatarUrl: 'https://via.placeholder.com/40',
				// 	time: '2024-06-13 16:45',
				// 	content: 'Thanks for sharing this information.',
				// },
			],
			newComment: ''
		}
	},
	methods: {
		detail(index) {
			this.dormitoryID = this.tableData[index].dormitoryID;
			this.dormitoryName = this.tableData[index].dormitoryName;
			this.dormitoryDate = this.tableData[index].dormitoryDate;
			this.dormitoryPrice = this.tableData[index].dormitoryPrice;
			this.dormitoryAddress = this.tableData[index].dormitoryAddress;
			this.dormitoryPicture = this.tableData[index].dormitoryPicture;
			this.dormitoryAuthor = this.tableData[index].dormitoryAuthor;
			this.dormitoryDesc = this.tableData[index].dormitoryDesc;
			this.dormitoryStar = parseFloat(this.tableData[index].dormitoryStar);
		},
		getCommentList() {
			axios({
				method: 'get',
				url: 'http://localhost:8080/getCommentList',
				headers: {
					'Content-type': 'application/x-www-form-urlencoded'
				},
				data: qs.stringify({
					userID: this.userID,
					dormitoryID: this.dormitoryID
				}),
			}).then(function (response) {
				_this.comments = response.data.dataList
				console.log(_this.comments)
			}).catch(function (error) {
				console.log(error)
			})
		},
		addComment() {
			console.log(this.newComment)
			if (this.newComment) {
				if (this.curUser.userID && this.curUser.userID === '') {
					ele.Message.error("您未登录，正在跳转到登录界面");
					this.$router.push('/login')
				}
				axios({
					method: 'post',
					url: 'http://localhost:8080/sendComment',
					headers: {
						'Content-type': 'application/x-www-form-urlencoded'
					},
					data: qs.stringify(
						{
							userID: this.curUser.userID,
							dormitoryID: this.dormitoryID,
							comment: this.newComment
						}
					)
				}).then(function (response) {
					this.getCommentList()
					console.log(_this.tableData)
				}).catch(function (error) {
					console.log(error)
				})
			}
		},
		getDormitoryList(){
			axios({
				method: 'get',
				url: 'http://localhost:8080/getDormitaryList',
				headers: {
					'Content-type': 'application/x-www-form-urlencoded'
				}
			}).then(function (response) {
				_this.tableData = response.data.dataList
				this.detail(0).bind(this);
				console.log(this.tableData);
				this.getCommentList().bind(this);
			}).catch(function (error) {
				console.log(error)
			})
		}
	},
	created() {
		this.curUser.userID = window.localStorage.getItem('userID');
		this.curUser.userName = window.localStorage.getItem('userName');

		const mockData = {
			"code": 0,
				"msg": "ok",
				"data": [
				{
					"dormitoryID": 2,
					"dormitoryName": "1",
					"dormitoryDesc": "1",
					"dormitoryAddress": "1",
					"dormitoryPrice": 1,
					"dormitoryStar": 1,
					"dormitoryPhone": "1",
					"dormitoryRemain": 1,
					"dormitoryPicture": "1",
					"dormitoryAuthor": "1",
					"createdAt": "2024-06-24T08:27:11+08:00",
					"updatedAt": "2024-06-24T08:27:11+08:00"
				}
			]
		}

		this.tableData = mockData.data
		this.detail(0)
		console.log(this.tableData);


		// const mockData = {
		// 	"status": "success",
		// 	"dataList": [
		// 		{
		// 			"dormitoryID": "1",
		// 			"dormitoryName": "Sunny Retreat",
		// 			"dormitoryDate": "2024-06-15",
		// 			"dormitoryAddress": "123 Sunlight St, Sunnyvale",
		// 			"dormitoryPrice": "$150 per night",
		// 			"dormitoryDesc": "A cozy and sunny retreat perfect for relaxation.",
		// 			"dormitoryAuthor": "Jane Doe",
		// 			"dormitoryStar": "4.5",
		// 			"dormitoryPicture": "https://th.bing.com/th/id/R.d53ab0005d83bbc3f2d8c12c7ee21b97?rik=%2fIYrl5jFQKqAJA&riu=http%3a%2f%2fseopic.699pic.com%2fphoto%2f50036%2f0204.jpg_wh1200.jpg&ehk=R561g0BgKqnIJQccUg8x%2faFKzO5O5HfSPMCDa3YTsi8%3d&risl=&pid=ImgRaw&r=0"
		// 		},
		// 		{
		// 			"dormitoryID": "2",
		// 			"dormitoryName": "Mountain Haven",
		// 			"dormitoryDate": "2024-05-20",
		// 			"dormitoryAddress": "456 Mountain Rd, Hillside",
		// 			"dormitoryPrice": "$200 per night",
		// 			"dormitoryDesc": "Escape to the mountains in this beautiful haven.",
		// 			"dormitoryAuthor": "John Smith",
		// 			"dormitoryStar": "4.7",
		// 			"dormitoryPicture": "https://img.zcool.cn/community/01f23a5bc82cada801213dea4a5b3f.jpg@1280w_1l_2o_100sh.jpg"
		// 		},
		// 		{
		// 			"dormitoryID": "3",
		// 			"dormitoryName": "Beachside Bliss",
		// 			"dormitoryDate": "2024-06-01",
		// 			"dormitoryAddress": "789 Ocean Ave, Beach City",
		// 			"dormitoryPrice": "$250 per night",
		// 			"dormitoryDesc": "Enjoy the sea breeze and stunning views at Beachside Bliss.",
		// 			"dormitoryAuthor": "Alice Johnson",
		// 			"dormitoryStar": "4.8",
		// 			"dormitoryPicture": "https://th.bing.com/th/id/R.59048dfcc0ea2e183089246419e11b90?rik=BfOCYrwhFp4YxA&riu=http%3a%2f%2fseopic.699pic.com%2fphoto%2f50036%2f0236.jpg_wh1200.jpg&ehk=57PvzqCzkp9KyI1VpGTsjLMbjcShj7x7vsiS2B9KsIw%3d&risl=&pid=ImgRaw&r=0"
		// 		},
		// 		{
		// 			"dormitoryID": "4",
		// 			"dormitoryName": "Urban Oasis",
		// 			"dormitoryDate": "2024-04-15",
		// 			"dormitoryAddress": "101 City Rd, Metropolis",
		// 			"dormitoryPrice": "$180 per night",
		// 			"dormitoryDesc": "A tranquil oasis in the heart of the city.",
		// 			"dormitoryAuthor": "Bob Brown",
		// 			"dormitoryStar": "4.6",
		// 			"dormitoryPicture": "https://img.zcool.cn/community/01f23a5bc82cada801213dea4a5b3f.jpg@1280w_1l_2o_100sh.jpg"
		// 		}
		// 	]
		// }
		// this.tableData = mockData.dataList;
		// console.log(this.tableData);
		// this.detail(0);
		this.getDormitoryList()
	}
};
</script>

<style>
.dormitory-main {
	height: 100vh;
	display: flex;
	flex-direction: row;
}

.dormitory-left {
	height: 100vh;
	padding: 30px;
	overflow-y: auto;
}

.dormitory-right {
	height: 100vh;
	padding: 30px;
	overflow-y: auto;
}

.dormitory-card {
	padding: 0px;
	margin-bottom: 10px;
}

.dormitory-card th {
	border: none;
}

.dormitory-card thead tr th:nth-last-of-type(2) {
	border-right: 1px solid #EBEEF5;
}

.dormitory-card::before {
	width: 0;
}

.desc-img {
	width: 100%;
}

.grid-cont-right {
	text-align: left;
	padding-left: 30px;
	font-size: 14px;
	color: black;
}

.grid-cont-right h2 {
	color: #42b983;
}

.comments-section {
	margin-top: 50px;
}

.comment-card {
	margin-bottom: 20px;
	border: 1px solid #ebebeb;
	border-radius: 10px;
}

.comment-header {
	display: flex;
	align-items: center;
	padding: 10px;
	border-bottom: 1px solid #ebebeb;
}

.comment-info {
	margin-left: 10px;
}

.comment_place_text {
	margin-top: 0px;
	margin-bottom: 0px;
}

.comment-info h3 {
	margin: 0;
	color: #333;
	text-align: left;
	margin-left: 5px;
}

.comment-time {
	margin: 0;
	color: #999;
	font-size: 12px;
	text-align: left;
	margin-top: 5px;
	margin-left: 5px;
}

.comment-body {
	padding: 10px;
	text-align: left;
}

.comment-body p {
	margin: 0;
	color: #555;
	line-height: 1.6;
}

.comment-form {
	display: flex;
	flex-direction: column;
	gap: 10px;
}

</style>
