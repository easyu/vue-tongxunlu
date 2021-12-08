<template>
  <div>
    <el-button type="text" @click="showSave = true">新增</el-button>
    <el-dialog title="添加通讯录" :visible.sync="showSave" width="48%">
      <el-form :inline="true" :model="formInline" class="demo-form-inline">
        <el-form-item label="姓名">
          <el-input v-model="formInline.name" placeholder="杰斯"></el-input>
        </el-form-item>
        <el-form-item label="手机号码">
          <el-input
            v-model="formInline.phone_number"
            placeholder="1888888888"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">保存</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <el-table
      :data="
        tableData.filter(
          (data) =>
            !search || data.name.toLowerCase().includes(search.toLowerCase())
        )
      "
      style="width: 100%"
    >
      <el-table-column label="姓名" prop="name"> </el-table-column>
      <el-table-column label="电话" prop="phone_number"> </el-table-column>
      <el-table-column align="right">
        <template slot-scope="scope">
          <el-button size="mini" @click="update(scope.$index, scope.row)"
            >修改</el-button
          >
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  created() {
    this.findAll();
  },
  data() {
    return {
      formInline: {},
      tableData: [],
      showSave: false,
      search: "",
    };
  },
  methods: {
    update(index, row) {
      console.log(index, row);
      this.$axios
        .post("/addressBook/update", row)
        .then((this.showSave = false));
    },
    handleDelete(index, row) {
      console.log(index, row);
      this.$axios
        .get("/addressBook/delete?id=" + row.AddressBookId)
        .then((this.showSave = false));
    },
    saveOne(row) {
      console.log(row);
    },
    onSubmit() {
      this.$axios
        .post("/addressBook/save", this.formInline)
        .then((this.showSave = false));
    },
    findAll() {
      this.$axios.get("/addressBook/findAll").then((res) => {
        this.tableData = res.data.result;
      });
    },
  },
};
</script>

