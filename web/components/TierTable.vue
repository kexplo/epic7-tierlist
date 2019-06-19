<template>
  <el-card class="box-card">
    <div slot="header" class="clearfix">
      <el-row>
        <el-col :xs="24" :sm="6" :md="4">
          <span>{{ title }}</span>
        </el-col>
        <el-col :xs="24" :sm="8" :md="{offset: 11, span:6}">
          <el-input v-model="search" style="float: right; padding: 3px 0" size="small" placeholder="Type to search" />
        </el-col>
        <el-col :xs="8" :sm="{offset: 1, span: 6}" :md="{offset: 1, span:2}">
          <el-select v-model="pageSize" placeholder="Select">
            <el-option
              v-for="item in [5, 10, 15, 20]"
              :key="item"
              :label="item"
              :value="item">
            </el-option>
          </el-select>
        </el-col>
      </el-row>
    </div>

    <el-table
      :data="filtered_items.slice((page-1)*pageSize, page*pageSize)"
      size="small"
      @sort-change="sort_change"
    >
      <template v-for="header in headers">
        <el-table-column
          v-if="header.type == 'img'"
          :key="header.name"
          :prop="header.value"
          :label="header.text"
          :width="header.width"
          :fixed="header.fixed"
        >
          <template v-slot:default="scope" :row_key="key">
            <el-image :src="scope.row[header.value]" fit="fill" />
          </template>
        </el-table-column>
        <el-table-column
          v-else-if="header.sortable"
          :key="header.name"
          :prop="header.value"
          :label="header.text"
          :width="header.width"
          sortable="custom"
        />
        <el-table-column
          v-else
          :key="header.name"
          :prop="header.value"
          :label="header.text"
          :width="header.width"
        />
      </template>
    </el-table>

    <el-divider />

    <el-pagination
      layout="prev, pager, next"
      :page-size="pageSize"
      :total="filtered_items.length"
      @current-change="current_change"
    />

    <!--
    <el-table
      :data="items"
    >
      <template v-for="header in headers">
        <el-table-column v-if="header.value == 'img'" :key="header.name" :prop="header.value" :label="header.text">
          <template slot-scope="scope">
            <el-image :src="scope.row.img" />
          </template>
        </el-table-column>
        <el-table-column v-else :key="header.name" :prop="header.value" :label="header.text">
        </el-table-column>
      </template>
    </el-table>
    -->
  </el-card>
</template>

<script>
export default {
  name: 'TierTable',
  props: {
    headers: {
      type: Array,
      default: () => []
    },
    items: {
      type: Array,
      required: true,
      default: () => [] },
    pagination: {
      type: Object,
      default: () => {}
    },
    search: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: ''
    },
    pageSize: {
      type: Number,
      default: 10
    }
  },
  data: function() {
    return {
      page: 1,
      sortKey: null,
      sortOrder: null,
    }
  },
  computed: {
    filtered_items: function() {
      let copied_array = [...this.items];
      let sortKey = this.sortKey;
      // sortOrder: [descending, ascending, null]
      let sortOrder = this.sortOrder;
      if (sortKey != null && sortOrder != null) {
        copied_array.sort(function(a, b) {
          if (a[sortKey] > b[sortKey]) {
            return 1;
          }
          if (a[sortKey] < b[sortKey]) {
            return -1;
          }
          return 0;
        });
      }
      if (sortOrder == 'descending') {
        copied_array.reverse();
      }
      return copied_array;
    }
  },
  methods: {
    current_change: function (current_page) {
      this.page = current_page;
    },
    sort_change: function ({column, prop, order}) {
      this.sortKey = prop;
      this.sortOrder = order;
    }
  }
}
</script>
