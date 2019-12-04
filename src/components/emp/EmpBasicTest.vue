<template>
  <div>
    <el-container>

      <el-header style="padding: 0px;display:flex;justify-content:space-between;align-items: center">
        <div style="display: inline">
          <el-input
            placeholder="通过员工名搜索员工,记得回车哦..."
            clearable
            @change="keywordsChange"
            style="width: 300px;margin: 0px;padding: 0px;"
            size="mini"
            @keyup.enter.native="searchEmp"
            prefix-icon="el-icon-search"
            v-model="keywords">
          </el-input>
          <el-button type="primary" size="mini" style="margin-left: 5px" icon="el-icon-search" @click="searchEmp">搜索
          </el-button>
        </div>
      </el-header>

      <el-main style="padding-left: 0px;padding-top: 15px">
        <div>
          <el-table
            :data="emps"
            v-loading="tableLoading"
            border
            stripe
            size="mini"
            style="width: 100%">
            <el-table-column
              type="selection"
              align="left"
              width="30">
            </el-table-column>
            <el-table-column
              prop="name"
              align="left"
              fixed
              label="姓名"
              width="90">
            </el-table-column>
            <el-table-column
              prop="workID"
              width="85"
              align="left"
              label="工号">
            </el-table-column>
            <el-table-column
              prop="gender"
              label="性别"
              width="50">
            </el-table-column>
            <el-table-column
              width="85"
              align="left"
              label="出生日期">
              <template slot-scope="scope">{{ scope.row.birthday | formatDate}}</template>
            </el-table-column>
            <el-table-column
              prop="idCard"
              width="150"
              align="left"
              label="身份证号码">
            </el-table-column>
            <el-table-column
              prop="wedlock"
              width="70"
              label="婚姻状况">
            </el-table-column>
            <el-table-column
              width="50"
              prop="nation.name"
              label="民族">
            </el-table-column>
            <el-table-column
              prop="nativePlace"
              width="80"
              label="籍贯">
            </el-table-column>
            <el-table-column
              prop="politicsStatus.name"
              label="政治面貌">
            </el-table-column>
            <el-table-column
              prop="email"
              width="180"
              align="left"
              label="电子邮件">
            </el-table-column>
            <el-table-column
              prop="phone"
              width="100"
              label="电话号码">
            </el-table-column>
            <el-table-column
              prop="address"
              width="220"
              align="left"
              label="联系地址">
            </el-table-column>
            <el-table-column
              prop="department.name"
              align="left"
              width="100"
              label="所属部门">
            </el-table-column>
            <el-table-column
              width="100"
              align="left"
              prop="position.name"
              label="职位">
            </el-table-column>
            <el-table-column
              prop="jobLevel.name"
              width="100"
              align="left"
              label="职称">
            </el-table-column>
            <el-table-column
              prop="engageForm"
              label="聘用形式">
            </el-table-column>
            <el-table-column
              width="85"
              align="left"
              label="入职日期">
              <template slot-scope="scope">{{ scope.row.beginDate | formatDate}}</template>
            </el-table-column>
            <el-table-column
              width="85"
              align="left"
              label="转正日期">
              <template slot-scope="scope">{{ scope.row.conversionTime | formatDate}}</template>
            </el-table-column>
            <el-table-column
              width="95"
              align="left"
              label="合同起始日期">
              <template slot-scope="scope">{{ scope.row.beginContract | formatDate}}</template>
            </el-table-column>
            <el-table-column
              width="95"
              align="left"
              label="合同截至日期">
              <template slot-scope="scope">{{ scope.row.endContract | formatDate}}</template>
            </el-table-column>
            <el-table-column
              align="left"
              width="70"
              label="合同期限">
              <template slot-scope="scope">{{ scope.row.contractTerm}}年</template>
            </el-table-column>
            <el-table-column
              align="left"
              prop="tiptopDegree"
              label="最高学历">
            </el-table-column>
          </el-table>

          <div style="display: flex;justify-content: space-between;margin: 2px;padding-top: 15px">
            <el-pagination
              background
              :page-size="10"
              :current-page="currentPage"
              @current-change="currentChange"
              layout="prev, pager, next"
              :total="totalCount">
            </el-pagination>
          </div>
        </div>
      </el-main>
    </el-container>
  </div>

</template>

<script>
  export default {
    data()
    {
      return {
        emps: [],
        keywords: '',
        totalCount: -1,
        currentPage: 1,
        tableLoading: false
      }
    },
    mounted: function ()
    {
      this.loadEmpsTest();
    },

    methods: {
      currentChange(currentChange)
      {
        this.currentPage = currentChange;
        this.loadEmpsTest();
      },

      keywordsChange(val) {
        if (val == '') {
          this.loadEmpsTest();
        }
      },
      searchEmp() {
        this.loadEmpsTest();
      },

      loadEmpsTest()
      {
        var _this = this;
        this.tableLoading = true;
        this.getRequest("/employee/basic/empTest?page=" + this.currentPage + "&size=10&keywords=" + this.keywords).then(resp =>
        {
          this.tableLoading = false;
          if (resp && resp.status == 200)
          {
            var data = resp.data;
            _this.emps = data.emps;
            _this.totalCount = data.count;
          }
        })
      }
    }

  }
</script>
