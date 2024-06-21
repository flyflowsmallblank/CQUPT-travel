<template>
<div class="hotel-main">
    <el-col :span="23" class="mesgList-table">
        <el-table :data="tableData.slice((currentPage - 1)* pageSize, currentPage * pageSize)" style="width: 100%">
            <el-table-column label="编号" width="100px">
                <template slot-scope="scope">
                    <span>{{ scope.row.mesgID }}</span>
                </template>
            </el-table-column>
            <el-table-column label="公告标题" width="300px">
                <template slot-scope="scope">
                    <span>{{ scope.row.mesgTitle }}</span>
                </template>
            </el-table-column>
            <el-table-column label="详细信息" :span="5">
                <template slot-scope="scope">
                    <span>{{ scope.row.mesgDesc }}</span>
                    <el-button style="margin-left: 10px;" size="mini" type="text" @click="showDetail(scope.$index)">查看详情</el-button>
                </template>
            </el-table-column>
            <el-table-column label="发布者" width="100px">
                <template slot-scope="scope">
                    <span>{{ scope.row.mesgAuthor }}</span>
                </template>
            </el-table-column>
            <el-table-column label="发布时间" width="200px">
                <template slot-scope="scope">
                    <span>{{ scope.row.mesgDate }}</span>
                </template>
            </el-table-column>
        </el-table>
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage" :page-sizes="[5, 10]" :page-size="pageSize" background layout="total, sizes, prev, pager, next, jumper" :total="tableData.length" style="padding-top:20px">
        </el-pagination>
    </el-col>
    <el-dialog title="公告详情" :visible.sync="dialogVisible" width="30%">
        <h2>{{ msgTitle }}</h2>
        <span style="color:rgb(64,158,255)">{{ msgAuthor }} </span>
        <span style="color:gray">{{ msgDate }}</span>
        <p>{{ msgDesc }}</p>
        <span slot="footer" class="dialog-footer">
            <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
        </span>
    </el-dialog>
</div>
</template>

<script>
const axios = require('axios')
const qs = require('qs')
const ele = require('element-ui')

export default {
    name: 'hotel_page',
    data() {
        return {
            currentPage: 1,
            pageSize: 5,
            dialogVisible: false,
            msgTitle: '',
            msgDesc: '',
            msgDate: '',
            msgAuthor: '',
            tableData: []
        }
    },
    methods: {
        // 初始页currentPage、初始每页数据数pagesize和数据data
        handleSizeChange: function (size) {
            this.pageSize = size;
            console.log(this.pageSize) //每页下拉显示数据
        },
        handleCurrentChange: function (currentPage) {
            this.currentPage = currentPage;
            console.log(this.currentPage) //点击第几页
        },
        showDetail(index) {
            this.msgTitle = this.tableData[index].mesgTitle
            this.msgDate = this.tableData[index].mesgDate
            this.msgDesc = this.tableData[index].mesgDesc
            this.msgAuthor = this.tableData[index].mesgAuthor
            this.dialogVisible = true
        },

    },
    created() {
        // let _this = this
        // this.userName = window.localStorage.getItem('userName')
        // axios({
        //     method: 'get',
        //     url: 'http://localhost:8080/getAnnouncementList',
        //     headers: {
        //         'Content-type': 'application/x-www-form-urlencoded'
        //     }
        // }).then(function (response) {
        //     _this.tableData = response.data.mesgList
        // }).catch(function (error) {
        //     console.log(error)
        // })

		// Mock data
		this.tableData = [
			{
				mesgID: "1",
				mesgTitle: "New Features Release",
				mesgDate: "2024-06-15",
				mesgAuthor: "Admin",
				mesgDesc: "We are excited to announce new features..."
			},
			{
				mesgID: "2",
				mesgTitle: "Maintenance Downtime",
				mesgDate: "2024-06-10",
				mesgAuthor: "Admin",
				mesgDesc: "Scheduled maintenance on June 20th..."
			},
			{
				mesgID: "3",
				mesgTitle: "Community Guidelines Update",
				mesgDate: "2024-06-05",
				mesgAuthor: "Admin",
				mesgDesc: "We have updated our community guidelines..."
			},
			{
				mesgID: "4",
				mesgTitle: "Summer Event",
				mesgDate: "2024-06-01",
				mesgAuthor: "Admin",
				mesgDesc: "Join us for the summer event on July 1st..."
			},
			{
				mesgID: "5",
				mesgTitle: "Bug Fixes and Improvements",
				mesgDate: "2024-05-25",
				mesgAuthor: "Admin",
				mesgDesc: "We have fixed several bugs and made improvements..."
			},
			{
				mesgID: "6",
				mesgTitle: "New Partnership Announcement",
				mesgDate: "2024-05-20",
				mesgAuthor: "Admin",
				mesgDesc: "We are pleased to announce a new partnership..."
			}
		];
    }
};
</script>

<style>
.hotel-main {
    height: 100%;
    padding: 30px;
    margin-top: 20px;
}

.grid-cont-right h2 {
    color: #42b983;
}
</style>
