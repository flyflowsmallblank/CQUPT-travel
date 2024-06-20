<template>
<div class="home">
    <HomeCarousel class="home_banner" cards="visited" />
    <HomeWelcome msg="重囿欢迎你" msg_en="Welcome to CQUPT" />
</div>
</template>

<script>
// @ is an alias to /src
import HomeWelcome from '@/components/HomeWelcome.vue'
import HomeCarousel from '@/components/HomeCarousel.vue'

export default {
    name: 'Home',
    components: {
        HomeCarousel,
        HomeWelcome
    }
}
</script>

<style>
body {
    padding: 0;
    margin: 0;
}

html,
body,
#app {
    text-align: left;
}

.home-logo {
    padding-top: 40px;
    padding-bottom: 40px;
    height: 80px;
}

.home {
    text-align: center;
}

.container{
	display: flex;
	align-items: center;
	justify-content: center;
}

.container span{
	font-size: 50px;
	padding-left: 30px;
}

.home_banner{
	margin-top: 70px;
}

</style>
