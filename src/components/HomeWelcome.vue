<template>
	<div class="hello">
		<img class="home_bottom_head_img" src="../assets/logo.png" alt="CQUPT">
		<p class="home_bottom_head_title">{{ msg }}</p>
		<p style="color:#02b983">{{ msg_en }}</p>
		<p>
			让学校信息查询数字化
		</p>
		<p>
			Digitize school information inquiries
		</p>
		<br>
		<p>
			游客可以快速浏览学校的景点，提前做出规划
		</p>
		<p>
			学生可以轻松浏览宿舍照片和及时获取生活资讯
		</p>
		<p>
			管理员可以高效处理宿舍公告，查看宿舍信息，管理景点信息等等
		</p>
		<p>
			Tourists can quickly browse the school's attractions and plan ahead
		</p>
		<p>
			Students can easily browse dormitory distribution and promptly get life updates
		</p>
		<p>
			Administrators can efficiently handle dormitory announcements, view dormitory information, manage sightseeing information, and more
		</p>
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
	}
}
</script>

<style lang="scss" scoped>
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

.home_bottom_head_title {
	font-size: 50px;
	margin-bottom: 0px;
	padding-bottom: 0px;
	margin-top: 0px;
}

.home_bottom_head_img{
	width: 150px;
	height: 150px;
	margin-top: 15pxF;
}
</style>
