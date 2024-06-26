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

.home {
    text-align: center;
	background: linear-gradient(to bottom right, rgba(255, 241, 235, 0.5) 0%, rgba(172, 224, 249, 0.5) 100%);

}

.container span{
	font-size: 50px;
	padding-left: 30px;
}

.home_banner{
	margin-top: 50px;
}

</style>
