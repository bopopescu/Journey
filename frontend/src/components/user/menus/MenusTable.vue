<template>
    <div class="menustable">
        <el-table
        size="small"
        :data="TableData.slice((currentPage-1)*pagesize,currentPage*pagesize)"
        :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
        row-key="id"
        border
        strip>
            <el-table-column prop="name" :label="TableColumn.name"></el-table-column>
            <el-table-column prop="mtype" :label="TableColumn.mtype">
                <template slot-scope="scope">
                    <el-tag size="small" v-if="scope.row.mtype == 0">目录</el-tag>
                    <el-tag size="small" v-else-if="scope.row.mtype == 1" type="warning">菜单</el-tag>
                    <el-tag size="small" v-else-if="scope.row.mtype == 2" type="success">子路由</el-tag>
                </template>
            </el-table-column>
            <el-table-column prop="icon" :label="TableColumn.icon">
                <template slot-scope="scope">
                    <icon-svg style="width: 17px;height: 17px;" :iconClass="scope.row.icon"></icon-svg>
                </template>
            </el-table-column>
            <el-table-column prop="url" :label="TableColumn.url"></el-table-column>
            <!-- <el-table-column prop="perms" :label="TableColumn.perms"></el-table-column> -->
            <el-table-column prop="del_flag" :label="TableColumn.del_flag">
                <template slot-scope="scope">
                    <el-tag size="small" style="color:#13c2c2;background-color:#e6fffb;border-color:#87e8de" v-if="scope.row.del_flag == 0">正常</el-tag>
                    <el-tag size="small" v-else-if="scope.row.del_flag == -1" type="info">禁用</el-tag>
                </template>
            </el-table-column>
            <el-table-column width="300em;" align="center" label="操作">
            <template slot-scope="scope">
                <el-button @click="handleData(scope.$index,scope.row,'edit')" size="mini" type="primary">编辑</el-button>
                <el-button v-if="scope.row.del_flag == 0" @click="handleData(scope.$index,scope.row,'disable')" size="mini" type="danger">禁用</el-button>
                <el-button v-else-if="scope.row.del_flag == -1" @click="handleData(scope.$index,scope.row,'enable')" size="mini" type="success">启用</el-button>
            </template>
            </el-table-column>
        </el-table>
        <el-pagination
        background
        layout="total, prev, pager, next, jumper"
        :total="TableData.length"
        :page-size="pagesize"
        @current-change="current_change">
        </el-pagination>
    </div>
</template>

<script>
export default {
    name: 'menustable',
    data () {
        return {
            // 分页参数
            total: 0,
            currentPage: 1,
            pagesize: 15,
        }
    },
    props: {
        TableData: {
            type: Array
        },
        TableColumn: {
            type: Object
        },
        delData: {
            type: Function
        },
        editData: {
            type: Function
        },
    },
    methods: {
        current_change(currentPage){
            this.currentPage = currentPage;
        },
        handleData(index,row,type) {
            if (type == 'edit') {
                let req_data = {'id':row.id}
                this.editData(req_data,'content')
            }
            else if (type == 'disable') {
                let req_data = {'id':row.id,'del_flag':-1}
                this.editData(req_data,'status')
            }
            else if (type == 'enable') {
                let req_data = {'id':row.id,'del_flag':0}
                this.editData(req_data,'status')
            }
        },
    }
}
</script>

<style>

</style>