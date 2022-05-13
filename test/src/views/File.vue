<template>
  <div class="files">
    <div style="padding:0 auto">
      <el-table
        :data="tableData"
        style="width: 100%;"
        height="500"
        @row-click="handleRowClick"
      >
        <el-table-column fixed prop="name" label="文件名" width="600">
        </el-table-column>
        <el-table-column prop="createdate" label="创建日期" width="200">
        </el-table-column>
        <el-table-column
          prop="modifiedate"
          label="最后修改日期"
          width="200"
        ></el-table-column>
        <el-table-column width="300">
          <template slot-scope="scope">
            <el-button
              type="text"
              size="small"
              style="padding: 0 0"
              @click="handleClick(scope.row)"
              >编辑</el-button
            >
          </template></el-table-column
        >
      </el-table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [
        {
          name: "1",
          createdate:"2016-05-04",
          modifiedate:"2016-05-05",
        },
        {
          name: "2",
          createdate:"2016-05-04",
          modifiedate:"2016-05-05",
        },
        {
          name: "3",
          createdate:"2016-05-04",
          modifiedate:"2016-05-05",
        },
        {
          name: "4",
          createdate:"2016-05-04",
          modifiedate:"2016-05-05",
        },
        {
          name: "5",
          createdate:"2016-05-04",
          modifiedate:"2016-05-05",
        },
      ],
    };
  },
  methods: {
    handleClick(row) {
      // console.log(row);
      this.$router.push({ name: "editor", params: { openFile: row.name }}).catch((error) => error);
    },
    handleRowClick(row, column, event) {
        column;event;
        // console.log(row);
        this.$router.push({ name: "editor", params: { openFile: row.name }}).catch((error) => error);
    }
  },
};
</script>