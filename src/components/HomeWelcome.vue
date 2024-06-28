<template>
	<div class="hello">
		<img class="home_bottom_head_img" src="../assets/logo.png" alt="CQUPT">
		<p class="home_bottom_head_title">{{ animatedText }}</p>
		<p style="color:#02b983">{{ msg_en }}</p>
		<p style="margin-top: 40px">
			让学校信息查询数字化
		</p>
		<p style="margin-bottom:0">
			Digitize school information inquiries
		</p>
		<br>
		<div class="advantage" style="margin-top: 0">
			<div class="advantage_item1">
				<img src="../assets/home_column1.jpg" alt="first_img">
				<p>
					游客可以快速浏览学校的景点，提前做出规划
				</p>
				<p>
					Tourists can quickly browse the school's attractions and plan ahead
				</p>
			</div>
			<div class="advantage_item2">
				<img src="../assets/home_column2.jpg" alt="second_img">
				<p>
					学生可以轻松浏览宿舍照片和及时获取生活资讯
				</p>
				<p>
					Students can easily browse dormitory distribution and promptly get life updates
				</p>
			</div>
			<div class="advantage_item3">
				<img src="../assets/home_column3.jpg" alt="third_img">
				<p>
					管理员可以高效处理宿舍公告，查看宿舍信息，管理景点信息等等
				</p>
				<p>
					Administrators can efficiently handle dormitory announcements, view dormitory information, manage
					sightseeing information, and more
				</p>
			</div>
		</div>
		<br>
		<p>
			重囿让校园生活更加便捷与温馨
		</p>
		<p style="color:#42b983">
			Like a warm ray of sunshine, it illuminates every student's campus life, bringing convenience and warmth
		</p>
	</div>
</template>

<script>
export default {
	name: 'HomeWelcome',
	props: {
		msg: String,
		msg_en: String
	},
	data() {
		return {
			animatedText: ""
		};
	},
	mounted() {
		this.animateText();
	},
	methods: {
		animateText() {
			const text = this.msg;
			let index = 0;
			const intervalId = setInterval(() => {
				this.animatedText = text.slice(0, index + 1);
				index++;
				if (index > text.length) {
					clearInterval(intervalId);
				}
			}, 300); // Adjust the speed of animation here (in milliseconds)
		}
	},
}
</script>

<style lang="scss" scoped>
.advantage {
	display: flex;
	justify-content: space-around;
	margin: 20px;
	padding: 20px;
	border-radius: 10px;
	box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
	text-align: center;
}

.advantage div {
	flex: 1;
	img {
		max-width: 80%;
		height: 600px;
		border-radius: 10px;
	}
}

h3 {
	margin: 40px 0 0;
}

ul {
	list-style-type: none;
	padding: 0;
}

li {
	display: inline-block;
	margin: 0 10px;
}

a {
	color: #42b983;
}

.hello {
	padding-bottom: 50px;
}

.home_bottom_head_img {
	width: 150px;
	height: 150px;
	margin-top: 30px;
	margin-bottom: 20px;
}


.home_bottom_head_title {
	font-size: 50px;
	margin-bottom: 0px;
	padding-bottom: 0px;
	margin-top: 0px;
	animation: fadeIn 4s ease forwards;
	opacity: 0;
}

@keyframes fadeIn {
	from {
		opacity: 0;
	}
	to {
		opacity: 1;
	}
}
</style>
