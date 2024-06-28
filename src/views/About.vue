<template>
	<div class="about">
		<div class="about-header">
			<h1>关于我们</h1>
			<p>欢迎来到重囿！</p>
		</div>
		<div class="about-content">
			<div v-for="(section, index) in sections" :key="index" class="about-section" :class="{ visible: ripple }" :style="{ animationDelay: `${index * 0.5}s` }">
				<h2 v-if="section.title">{{ section.title }}</h2>
				<p v-for="(paragraph, pIndex) in section.paragraphs" :key="pIndex">{{ paragraph }}</p>
				<ul v-if="section.list">
					<li v-for="(item, iIndex) in section.list" :key="iIndex"><strong>{{ item.title }}</strong>：{{ item.content }}</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'About',
	data() {
		return {
			ripple: false,
			sections: [
				{
					paragraphs: [
						"我们是一群热爱旅游与探索的重邮学子，怀揣着分享和发现的激情。我们致力于为大家打造一个最全面、最详细的重邮旅游信息平台。不论你是新生踏入重邮的第一步，还是初次来访这片地方的游客，我们的网站都将为您提供丰富的旅游指南和贴心建议，让您在这里的每一次旅程都充满无限可能。"
					]
				},
				{
					title: "我们的使命",
					paragraphs: [
						"我们的使命是通过分享有益的信息和真实的体验，让每一位游客都能在重邮度过愉快而充实的时光。我们致力于打造一个信息丰富、互动性强的平台，为大家提供最新、最全面的重邮旅游资讯。无论您是想探索校园的每个角落，还是寻找重邮美食，我们的网站将为您呈现最精彩的建议和实用的指南，助您留下难忘的重邮记忆。"
					]
				},
				{
					title: "我们的价值观",
					list: [
						{ title: "分享", content: "我们秉持着分享旅游经验和建议的理念，鼓励每一位游客和成员在我们的社区中互相交流，共同打造一个充满友善和互助的大家庭。" },
						{ title: "探索", content: "我们热爱探索未知，深入发掘重邮的每一个角落和每一个隐藏的美丽瞬间。我们相信每次探索都是一次心灵的成长和视野的开阔，愿与大家分享这些珍贵的发现和体验。" },
						{ title: "诚信", content: "我们坚持提供真实可靠的信息，致力于为每位游客和访问者带来最真实的重邮体验。无论是旅游景点的介绍、交通指南还是文化背景，我们都保证信息的准确性和客观性。" },
						{ title: "合作", content: "我们珍视团队合作的力量，欢迎所有热爱旅游的朋友加入我们的团队，共同探索、共同成长。通过集体智慧和协作精神，我们致力于为社区带来更多创新和价值，让重邮的每一位访客都能享受到更丰富的旅游体验。" }
					]
				},
				{
					title: "我们的内容",
					paragraphs: [
						"我们的网站致力于为您提供全面的重邮及周边旅游信息，涵盖景点介绍、路线推荐、美食指南、住宿信息以及各类实用旅游贴士。不论您是寻觅最佳的拍照打卡地，还是探索偏远的徒步路线，我们都精心准备了丰富而有价值的内容，旨在为您的旅行提供最好的参考和指导。无论是短暂的访问还是深入的探索，我们都希望通过我们的平台，让您的每一次旅程都充满着令人难忘的经历和宝贵的回忆。"
					]
				},
				{
					title: "联系我们",
					paragraphs: [
						"如果你有任何问题或建议，欢迎随时联系我们。我们期待与你分享更多的旅游故事和经验！",
						"邮箱： 2669555225@qq.com",
						"地址： 重庆市南岸区崇文路2号重庆邮电大学",
						"感谢你对重囿的关注与支持！"
					]
				}
			]
		};
	},
};
</script>

<style scoped>
.about {
	position: relative;
	display: flex;
	flex-direction: column;
	align-items: center;
	padding-left: 200px;
	padding-right: 200px;
	border-radius: 10px;
	box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
	font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
	overflow: hidden;
	background-image: linear-gradient(to right, rgba(255, 149, 105, 0.5) 0%, rgba(233, 39, 88, 0.5) 100%);
	background-size: 400% 400%;
	animation: gradientAnimation 10s ease infinite;
}

.about-header {
	text-align: center;
	margin-bottom: 30px;
}

.about-header h1 {
	color: #2c3e50;
	margin-bottom: 10px;
	font-size: 2.5em;
	font-weight: bold;
}

.about-header p {
	color: #666;
	font-size: 1.2em;
	margin-top: 0;
}

.about-content {
	width: 100%;
	text-align: left;
}

.about-section {
	background: rgba(255, 255, 255, 0.8);
	padding: 20px;
	margin-bottom: 20px;
	border-radius: 10px;
	opacity: 0;
	transform: translateY(20px);
	animation: fadeInUp 1s forwards;
}

.about-section.visible {
	opacity: 1;
	transform: translateY(0);
}

.about-content h2 {
	color: #2c3e50;
	border-left: 5px solid #2c3e50;
	padding-left: 10px;
	margin-top: 0;
	margin-bottom: 15px;
	font-size: 1.8em;
	font-weight: bold;
}

.about-content p,
.about-content li {
	color: #666;
	line-height: 1.8;
	margin: 10px 0;
	font-size: 1.1em;
}

.about-content ul {
	list-style-type: disc;
	padding-left: 40px;
}

.about-content ul li {
	margin: 10px 0;
}

.about-content p {
	text-align: justify;
}

@keyframes gradientAnimation {
	0% {
		background-position: 0 50%;
	}
	50% {
		background-position: 100% 50%;
	}
	100% {
		background-position: 0 50%;
	}
}

@keyframes fadeInUp {
	from {
		opacity: 0;
		transform: translateY(20px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}
</style>
