<template>
    <div class="class-list">
        <h1 class="class-list-title">{{title}}</h1>
        <el-table v-if="classList.length>0" @cell-dblclick="clickCell" :data="classList[index]" border :span-method="objectSpanMethod" v-for="(classDetail,index) of classList" :key="index">
            <el-table-column :prop="item.prop" :label="item.label" v-for="(item,index) of heads" :key="index">
                <!-- :class="index === currentIndex?'first':''" -->
            </el-table-column>
        </el-table>
        <h2 v-if="classList.length===0">暂无课程</h2>

        <classDetail @close="closeDetail" :dialogVisible="dialogVisible" :lessonId="lessonId" @delClass="delClass"></classDetail>
        <el-select v-model="schoolId" placeholder="请选择要查看的校区" class="school-select">
            <el-option v-for="(item,index) in schoolList" :key="index" :label="item.label" :value="item.value">
            </el-option>
        </el-select>
    </div>
</template>
<style lang="css">
.class-list thead {
    display: none !important;
    text-align: center;
}
.class-list div:nth-of-type(1) thead {
    display: table-header-group !important;
}
.class-list .el-table__body-wrapper {
    margin-top: 0px;
}
.school-select {
    width: 900px;
    margin: 20px auto;
}
</style>

<script>
import { getSchool, getClassDetail } from '../api/getData1';
import ClassDetail from './subpages/ClassDetail';
export default {
    components: {
        ClassDetail
    },
    async created() {
        this.getSchoolList();
    },
    data() {
        return {
            currentIndex: 0,
            dialogVisible: false,
            lessonId: null,
            ids: [],
            schoolList: [],
            schoolId: '',
            // 课程表名称
            title: null,
            // 表头字段
            heads: [],
            // 班级列表
            classList: []
        };
    },
    watch: {
        schoolId() {
            this.getClass();
        },
        $route() {
            this.getClass();
        }
    },
    methods: {
        delClass() {
            this.getClass();
        },
        closeDetail() {
            this.dialogVisible = false;
        },
        async getSchoolList() {
            // 获取校区
            const res = await getSchool();
            if (res.ok) {
                console.log('成功请求校区列表');
                res.list.forEach(item => {
                    var temp = {
                        value: item.id,
                        label: item.name
                    };
                    this.schoolList.push(temp);
                });
                this.schoolId = this.schoolList[0].value;
            }
        },
        async getClass() {
            const res = await getClassDetail({
                schoolId: this.schoolId
            });
            console.log(res);
            if (res.ok) {
                console.log('成功请求课程表');
                this.title = res.title;
                this.heads = res.heads;

                res.list.forEach(item => {
                    item.forEach(subItem => {
                        subItem.id.forEach(xItem => {
                            xItem.lessonName =
                                xItem.lessonName + '-' + xItem.lessonId;
                        });
                        this.ids = this.ids.concat(subItem.id);
                    });
                });
                this.classList = res.list;
            }
        },
        clickCell(row, column, cell, event) {
            console.log(row, column, cell);
            const tempName = cell.innerHTML.replace('<div class="cell">', '');
            const className = tempName.replace('</div>', '');

            let weekday;
            switch (row.weekday) {
                case '周一':
                    weekday = '1';
                    break;
                case '周二':
                    weekday = '2';
                    break;
                case '周三':
                    weekday = '3';
                    break;
                case '周四':
                    weekday = '4';
                    break;
                case '周五':
                    weekday = '5';
                    break;
                case '周六':
                    weekday = '6';
                    break;
                case '周日':
                    weekday = '7';
                    break;
            }
            const query = {
                weekday,
                startTime: row.startTime,
                endTime: row.endTime,
                location: column.label,
                lessonName: row[column.property]
            };
            // 如果点击空课程表，跳转到课程添加页面
            if (!className) {
                this.$router.push({
                    path: '/addClass',
                    query: query
                });
                return;
            }
            if (
                className.indexOf('周') > -1 ||
                className.indexOf('上午') > -1 ||
                className.indexOf('下午') > -1 ||
                className.indexOf('晚上') > -1
            ) {
                console.log(123);
            } else {
                console.log(45);
                // console.log(this.ids);
                this.ids.forEach(item => {
                    if (item.lessonName === className) {
                        this.lessonId = item.lessonId;
                        // console.log(this.lessonId);
                        this.dialogVisible = true;
                    }
                });
            }
        },
        selectClass(classId) {
            console.log(classId);
        },

        objectSpanMethod({ row, column, rowIndex, columnIndex }) {
            if (columnIndex === 0) {
                switch (rowIndex) {
                    case 0:
                        return {
                            rowspan: 10,
                            colspan: 1
                        };
                    case 1:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 2:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 3:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 4:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 5:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 6:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 7:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 8:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 9:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 10:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 11:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 12:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                    case 13:
                        return {
                            rowspan: 0,
                            colspan: 0
                        };
                }
            }
        }
    }
};
</script>