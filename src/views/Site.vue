<template>
<div class="site-main">
    <el-container>
        <el-container>
            <el-table :data="tableData" border style="width: 50%" class="site-card" :show-header="false" >
                <el-table-column :span="1">
<!--				slot—scope用于在父组件中访问子组件传递出来的特定数据-->
<!--				el-row用于创建网格布局-->
                    <template slot-scope="scope">
                        <el-row>
                            <el-col :span="8" >
								<div @click=detail(scope.$index)>
									<img class="desc-img" :src=scope.row.sitePicture />
								</div>
                            </el-col>
                            <el-col :span="16">
                                <div class="grid-cont-right"  @click=detail(scope.$index)>
                                    <h2>{{ scope.row.siteTitle }}</h2>
                                    <p><i style="color:#42b983;" class="el-icon-location">景点地址</i>&nbsp;&nbsp;{{ scope.row.siteCity }}</p>
                                </div>
                            </el-col>
                        </el-row>
                    </template>
                </el-table-column>
            </el-table>
            <el-footer style="width: 50%">到底了</el-footer>
        </el-container>
        <el-aside class="right_detail" width="50%">
            <el-card class="box-card">
                <div slot="header" class="clearfix">
                    <span>详情</span>
                </div>
                <div style="text-align:left;">
                    <h2>{{ this.siteTitle }}</h2>
                    <p><i style="color:#42b983;" class="el-icon-location">景点地址</i>&nbsp;&nbsp;&nbsp;{{ this.siteCity }}</p>
                    <p><i style="color:#42b983;" class="el-icon-s-ticket">景点介绍</i>&nbsp;&nbsp;&nbsp;{{ this.siteDesc }}</p>
                    <img :src=this.sitePicture slot="error" style="width:80%;" />
                </div>
            </el-card>
        </el-aside>
    </el-container>
</div>
</template>

<script>
const axios = require('axios')
const qs = require('qs')
const ele = require('element-ui')

export default {

    name: 'site_page',
    data() {
        return {
            siteID: '1',
            siteTitle: '',
            siteDate: '',
            siteCity: '',
            siteDesc: '',
            siteAuthor: '',
            siteStar: '',
            sitePicture: '',
            tableData: []
        }
    },
    methods: {
        detail(index) {
            this.siteID = this.tableData[index].siteID
            this.siteTitle = this.tableData[index].siteTitle
            this.sitePrice = this.tableData[index].sitePrice
            this.siteCity = this.tableData[index].siteCity
            this.sitePicture = this.tableData[index].sitePicture
            this.siteAuthor = this.tableData[index].siteAuthor
            this.siteDesc = this.tableData[index].siteDesc
            this.siteStar = this.tableData[index].siteStar
        }
    },
    created() {
        let _this = this
        this.userName = window.localStorage.getItem('userName')
        axios({
            method: 'get',
            url: 'http://115.159.4.245:8080/getSiteList',
            headers: {
                'Content-type': 'application/x-www-form-urlencoded'
            }
        }).then(function (response) {
            _this.tableData = response.data.data
            console.log(_this.tableData)
        }).catch(function (error) {
            console.log(error)
        })

		// const mockData = {
		// 	dataList: [
		// 		{
		// 			siteID: '1',
		// 			siteTitle: 'Beautiful Beach',
		// 			siteDate: '2024-06-18',
		// 			siteCity: 'Hawaii',
		// 			sitePrice: '$100',
		// 			siteDesc: 'A beautiful beach with white sand and clear blue water.',
		// 			siteAuthor: 'John Doe',
		// 			siteStar: '5',
		// 			sitePicture: 'https://th.bing.com/th/id/R.d53ab0005d83bbc3f2d8c12c7ee21b97?rik=%2fIYrl5jFQKqAJA&riu=http%3a%2f%2fseopic.699pic.com%2fphoto%2f50036%2f0204.jpg_wh1200.jpg&ehk=R561g0BgKqnIJQccUg8x%2faFKzO5O5HfSPMCDa3YTsi8%3d&risl=&pid=ImgRaw&r=0'
		// 		},
		// 		{
		// 			siteID: '2',
		// 			siteTitle: 'Mountain Adventure',
		// 			siteDate: '2024-06-19',
		// 			siteCity: 'Rocky Mountains',
		// 			sitePrice: '$200',
		// 			siteDesc: 'An exciting mountain adventure with breathtaking views.',
		// 			siteAuthor: 'Jane Smith',
		// 			siteStar: '4',
		// 			sitePicture: 'https://img.zcool.cn/community/01f23a5bc82cada801213dea4a5b3f.jpg@1280w_1l_2o_100sh.jpg'
		// 		},
		// 		{
		// 			siteID: '3',
		// 			siteTitle: 'Historical Museum',
		// 			siteDate: '2024-06-20',
		// 			siteCity: 'New York',
		// 			sitePrice: '$50',
		// 			siteDesc: 'A museum showcasing historical artifacts and exhibitions.',
		// 			siteAuthor: 'Alice Johnson',
		// 			siteStar: '5',
		// 			sitePicture: 'https://th.bing.com/th/id/R.59048dfcc0ea2e183089246419e11b90?rik=BfOCYrwhFp4YxA&riu=http%3a%2f%2fseopic.699pic.com%2fphoto%2f50036%2f0236.jpg_wh1200.jpg&ehk=57PvzqCzkp9KyI1VpGTsjLMbjcShj7x7vsiS2B9KsIw%3d&risl=&pid=ImgRaw&r=0'
		// 		},
		// 		{
		// 			siteID: '4',
		// 			siteTitle: 'Beautiful Beach',
		// 			siteDate: '2024-06-18',
		// 			siteCity: 'Hawaii',
		// 			sitePrice: '$100',
		// 			siteDesc: 'A beautiful beach with white sand and clear blue water.',
		// 			siteAuthor: 'John Doe',
		// 			siteStar: '5',
		// 			sitePicture: 'https://th.bing.com/th/id/R.d53ab0005d83bbc3f2d8c12c7ee21b97?rik=%2fIYrl5jFQKqAJA&riu=http%3a%2f%2fseopic.699pic.com%2fphoto%2f50036%2f0204.jpg_wh1200.jpg&ehk=R561g0BgKqnIJQccUg8x%2faFKzO5O5HfSPMCDa3YTsi8%3d&risl=&pid=ImgRaw&r=0'
		// 		},
		// 		{
		// 			siteID: '5',
		// 			siteTitle: 'Mountain Adventure',
		// 			siteDate: '2024-06-19',
		// 			siteCity: 'Rocky Mountains',
		// 			sitePrice: '$200',
		// 			siteDesc: 'An exciting mountain adventure with breathtaking views.',
		// 			siteAuthor: 'Jane Smith',
		// 			siteStar: '4',
		// 			sitePicture: 'https://img.zcool.cn/community/01f23a5bc82cada801213dea4a5b3f.jpg@1280w_1l_2o_100sh.jpg'
		// 		},
		// 		{
		// 			siteID: '6',
		// 			siteTitle: 'Historical Museum',
		// 			siteDate: '2024-06-20',
		// 			siteCity: 'New York',
		// 			sitePrice: '$50',
		// 			siteDesc: 'A museum showcasing historical artifacts and exhibitions.',
		// 			siteAuthor: 'Alice Johnson',
		// 			siteStar: '5',
		// 			sitePicture: 'https://th.bing.com/th/id/R.59048dfcc0ea2e183089246419e11b90?rik=BfOCYrwhFp4YxA&riu=http%3a%2f%2fseopic.699pic.com%2fphoto%2f50036%2f0236.jpg_wh1200.jpg&ehk=57PvzqCzkp9KyI1VpGTsjLMbjcShj7x7vsiS2B9KsIw%3d&risl=&pid=ImgRaw&r=0'
		// 		},
		// 		{
		// 			siteID: '7',
		// 			siteTitle: 'Beautiful Beach',
		// 			siteDate: '2024-06-18',
		// 			siteCity: 'Hawaii',
		// 			sitePrice: '$100',
		// 			siteDesc: 'A beautiful beach with white sand and clear blue water.',
		// 			siteAuthor: 'John Doe',
		// 			siteStar: '5',
		// 			sitePicture: 'https://th.bing.com/th/id/R.d53ab0005d83bbc3f2d8c12c7ee21b97?rik=%2fIYrl5jFQKqAJA&riu=http%3a%2f%2fseopic.699pic.com%2fphoto%2f50036%2f0204.jpg_wh1200.jpg&ehk=R561g0BgKqnIJQccUg8x%2faFKzO5O5HfSPMCDa3YTsi8%3d&risl=&pid=ImgRaw&r=0'
		// 		},
		// 		{
		// 			siteID: '8',
		// 			siteTitle: 'Mountain Adventure',
		// 			siteDate: '2024-06-19',
		// 			siteCity: 'Rocky Mountains',
		// 			sitePrice: '$200',
		// 			siteDesc: 'An exciting mountain adventure with breathtaking views.',
		// 			siteAuthor: 'Jane Smith',
		// 			siteStar: '4',
		// 			sitePicture: 'https://img.zcool.cn/community/01f23a5bc82cada801213dea4a5b3f.jpg@1280w_1l_2o_100sh.jpg'
		// 		},
		// 		{
		// 			siteID: '9',
		// 			siteTitle: 'Historical Museum',
		// 			siteDate: '2024-06-20',
		// 			siteCity: 'New York',
		// 			sitePrice: '$50',
		// 			siteDesc: 'A museum showcasing historical artifacts and exhibitions.',
		// 			siteAuthor: 'Alice Johnson',
		// 			siteStar: '5',
		// 			sitePicture: 'https://th.bing.com/th/id/R.59048dfcc0ea2e183089246419e11b90?rik=BfOCYrwhFp4YxA&riu=http%3a%2f%2fseopic.699pic.com%2fphoto%2f50036%2f0236.jpg_wh1200.jpg&ehk=57PvzqCzkp9KyI1VpGTsjLMbjcShj7x7vsiS2B9KsIw%3d&risl=&pid=ImgRaw&r=0'
		// 		}
		// 	]
		// };
		//
		// this.tableData = mockData.dataList;
		// console.log(this.tableData);
		// this.detail(0);
    }
};
</script>

<style>
.site-main {
    height: 100%;
    padding: 30px;
    margin-top: 20px;
}

.site-card {
    padding: 0px;
    margin-bottom: 10px;
    padding-right: 30px;
}

.site-card th {
    border: none;
}

.desc-img {
    width: 100%;
    padding-left: 0;
    background-color: lightslategray;
}

.grid-cont-right {
    text-align: left;
    padding-left: 30px;
    font-size: 14px;
    color: black;
    margin-top: -10px;
}

.grid-cont-right h2 {
    color: #42b983;
}

.right_detail{
	position: fixed;
	top: 60px;
	right: 0;
}
</style>




