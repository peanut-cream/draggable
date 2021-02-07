<template>
  <div style="width:800px">
    <button @click="add">add</button>
    <el-table :data="tableData" border row-key="id" align="left" ref="draptable">
       <el-table-column
                                    type="selection"
                                    width="50px" >
                                </el-table-column>
      <el-table-column prop="handle" label="操作">
        <template slot-scope="scope">
          <div>{{ scope.row.id }} <span class="handle">这里放图标</span> </div>
        </template>
      </el-table-column>
      <el-table-column prop="date" label="日期">
      </el-table-column>
      <el-table-column prop="name" label="姓名">
      </el-table-column>
      <el-table-column prop="address" label="地址">
      </el-table-column>

      <!-- <el-table-column
        v-for="(item, index) in dropCol"
        :key="`col_${index}`"
        :prop="dropCol[index].prop"
        :label="item.label"
      ></el-table-column> -->
    </el-table>
    <pre>
      {{tableData}}
    </pre>
  </div>
</template>
<script>
import Sortable from "sortablejs";
export default {
  data() {
    return {
      dropCol: [
        {
          label: "日期",
          prop: "date"
        },
        {
          label: "姓名",
          prop: "name"
        },
        {
          label: "地址",
          prop: "address"
        }
      ],
      tableData: [

      ],
      id:0,
    };
  },
  mounted() {
    this.rowDrop();
    // this.columnDrop();
  },
  methods: {
    add(){
      this.tableData.push({id:++this.id,date:'2020-02-10',name:"王"+this.id,address:"地址"})
    },
    //行拖拽
    rowDrop() {
    //   const tbody = document.querySelector(".el-table__body-wrapper tbody ");
      const tbody = this.$refs.draptable.$el.children[2].querySelector("tbody");
      const _this = this;
      Sortable.create(tbody, {
        animation: 180,
        delay: 0,
        handle:'.handle',
        onEnd(evt) {
            console.log(evt);
            let { newIndex, oldIndex }=evt;
            console.log(newIndex, oldIndex)
          const currRow = _this.tableData.splice(oldIndex, 1)[0];
          _this.tableData.splice(newIndex, 0, currRow);
        },
      });
    },
    //列拖拽
    columnDrop() {
      const wrapperTr = document.querySelector(".el-table__header-wrapper tr");
      this.sortable = Sortable.create(wrapperTr, {
        animation: 180,
        delay: 0,
        onEnd: evt => {
          const oldItem = this.dropCol[evt.oldIndex];
          this.dropCol.splice(evt.oldIndex, 1);
          this.dropCol.splice(evt.newIndex, 0, oldItem);
        }
      });
    }
  }
};
</script>
<style scoped lang="less">
.handle{
    cursor: move;
}
</style>
