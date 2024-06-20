<template>
<div class="dormitory-main">
    <el-container>
        <el-container>
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
                                <img class="desc-img" :src=scope.row.dormitoryPicture />
                            </el-col>
                            <el-col :span="16">
                                <div class="grid-cont-right" @click=detail(scope.$index)>
                                    <h2>{{ scope.row.dormitoryName }}</h2>
                                    <p><i style="color:#42b983;" class="el-icon-s-promotion">发布时间</i>&nbsp;&nbsp;{{ scope.row.dormitoryDate }}</p>
                                    <p><i style="color:#42b983;" class="el-icon-location">民宿地址</i>&nbsp;&nbsp;{{ scope.row.dormitoryAddress }}</p>
                                    <p><i style="color:#42b983;" class="el-icon-s-ticket">住宿价格</i>&nbsp;&nbsp;{{ scope.row.dormitoryPrice }}</p>
                                </div>
                            </el-col>
                        </el-row>
                    </template>
                </el-table-column>
            </el-table>
            <el-footer>到底了</el-footer>
        </el-container>
        <el-aside width="50%">
            <el-card class="box-card">
                <div slot="header" class="clearfix">
                    <span>民宿详情</span>
                </div>
                <div style="text-align:left;">
                    <h2>{{ this.dormitoryName }}</h2>
                    <el-rate v-model="dormitoryStar" disabled show-score text-color="#ff9900">
                    </el-rate>
                    <p><i style="color:#42b983;" class="el-icon-s-promotion">发布时间</i>&nbsp;&nbsp;&nbsp;{{ this.dormitoryDate }}</p>
                    <p><i style="color:#42b983;" class="el-icon-location">民宿地址</i>&nbsp;&nbsp;&nbsp;{{ this.dormitoryAddress }}</p>
                    <p><i style="color:#42b983;" class="el-icon-s-ticket">住宿价格</i>&nbsp;&nbsp;&nbsp;{{ this.dormitoryPrice }}</p>
                    <p><i style="color:#42b983;" class="el-icon-s-ticket">民宿介绍</i>&nbsp;&nbsp;&nbsp;{{ this.dormitoryDesc }}</p>
                    <img :src=this.dormitoryPicture style="width:80%;" />
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
    name: 'dormitory_page',
    data() {
        return {
            dormitoryID: '',
            dormitoryName: '',
            dormitoryDate: '',
            dormitoryAddress: '',
            dormitoryPrice: '',
            dormitoryDesc: '',
            dormitoryAuthor: '',
            dormitoryStar: 0,
            dormitoryPicture: '',
            tableData: []
        }
    },
    methods: {
        detail(index) {
            this.dormitoryID = this.tableData[index].dormitoryID
            this.dormitoryName = this.tableData[index].dormitoryName
            this.dormitoryDate = this.tableData[index].dormitoryDate
            this.dormitoryPrice = this.tableData[index].dormitoryPrice
            this.dormitoryAddress = this.tableData[index].dormitoryAddress
            this.dormitoryPicture = this.tableData[index].dormitoryPicture
            this.dormitoryAuthor = this.tableData[index].dormitoryAuthor
            this.dormitoryDesc = this.tableData[index].dormitoryDesc
            this.dormitoryStar = parseFloat(this.tableData[index].dormitoryStar)
        }
    },
    created() {
        let _this = this
        this.userName = window.localStorage.getItem('userName')
        axios({
            method: 'get',
            url: 'http://localhost:8080/getDormitoryList',
            headers: {
                'Content-type': 'application/x-www-form-urlencoded'
            }
        }).then(function (response) {
            _this.tableData = response.data.dataList
            console.log(_this.tableData)
        }).catch(function (error) {
            console.log(error)
        })
    }
};
</script>

<style>
.dormitory-main {
    height: 100%;
    padding: 30px;
}

.dormitory-card {
    padding: 0px;
    margin-bottom: 10px;
    padding-right: 30px;
}

.dormitory-card th {
    border: none;
}

.dormitory-card td,
.customer-table th.is-leaf {
    border: none;
}

.el-table--border,
.el-table--group {
    border: none;
}

.dormitory-card thead tr th.is-leaf {
    border: 1px solid #EBEEF5;
    border-right: none;
}

.dormitory-card thead tr th:nth-last-of-type(2) {
    border-right: 1px solid #EBEEF5;
}

.el-table--border::after,
.el-table--group::after {
    width: 0;
}

.dormitory-card::before {
    width: 0;
}

.dormitory-card .el-table__fixed-right::before,
.el-table__fixed::before {
    width: 0;
}

.el-table--border th.gutter:last-of-type {
    border: 1px solid #EBEEF5;
    border-left: none;
}

.el-header,
.el-footer {
    background-color: #f2f2f2;
    color: #333;
    text-align: center;
    margin-right: 30px;
    line-height: 60px;
    border-radius: 5px;
}

.el-aside {
    background-color: white;
    color: #333;
    text-align: center;
}

.el-main {
    background-color: white;
    color: #333;
    text-align: center;
}

.desc-img {
    width: 100%;
    padding-left: 0px;
}

.grid-content {
    display: flex;
    align-items: center;
    height: 100px;
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
</style>
