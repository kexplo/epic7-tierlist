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
              :value="item"
            />
          </el-select>
        </el-col>
      </el-row>
    </div>

    <el-table
      :data="filtered_items.slice((page-1)*pageSize, page*pageSize)"
      size="small"
      @sort-change="sort_change"
      @filter-change="filter_change"
    >
      <template v-for="header in headers">
        <el-table-column
          v-if="header.type == 'img'"
          :key="header.name"
          :column-key="header.value"
          :prop="header.value"
          :label="header.text"
          :width="header.width"
          :fixed="header.fixed"
          :filters="header.filters"
        >
          <template v-slot:default="scope" :row_key="key">
            <el-image :src="scope.row[header.value]" fit="fill" />
          </template>
        </el-table-column>
        <el-table-column
          v-else-if="header.sortable"
          :key="header.name"
          :column-key="header.value"
          :prop="header.value"
          :label="header.text"
          :width="header.width"
          :filters="header.filters"
          sortable="custom"
        />
        <el-table-column
          v-else
          :key="header.name"
          :column-key="header.value"
          :prop="header.value"
          :label="header.text"
          :width="header.width"
          :filters="header.filters"
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
      // filters: new Map(),
      filterMap: new Map(),
      filterUpdatedAt: null,  // trick variable
      search: '',
    }
  },
  computed: {
    filtered_items: function() {
      // let copied_array = [...this.items];
      let items = [...this.items];
      items = this.search_tierlist(items);
      items = this.filter_tierlist(items);
      this.sort_tierlist(items);
      return items;
    }
  },
  methods: {
    // handlers
    current_change: function (current_page) {
      this.page = current_page;
    },
    sort_change: function ({column, prop, order}) {
      this.sortKey = prop;
      this.sortOrder = order;
    },
    filter_change: function (filters) {
      let key = Object.keys(filters)[0];
      this.filterMap.set(key, filters[key]);
      // Vue.js does not support reactivity on `Map`.
      // By using 'filterUpdatedAt' variable, can trigger the computed method.
      this.filterUpdatedAt = new Date();
    },
    // methods
    search_tierlist: function (item_array) {
      if (this.search.length > 0) {
        return item_array.filter(x => x.name.toLowerCase().includes(this.search.toLowerCase()));
      }
      return item_array;
    },
    filter_tierlist: function (item_array) {
      if (!this.filterUpdatedAt) return item_array;

      return item_array.filter(x => {
        for (const [key, values] of this.filterMap) {
          if (values.length == 0) continue;
          if (!values.includes(x[key])) {
            return false;
          }
        }
        return true;
      });
    },
    sort_tierlist: function (item_array) {
      let sortKey = this.sortKey;
      // sortOrder: [descending, ascending, null]
      let sortOrder = this.sortOrder;
      if (sortKey != null && sortOrder != null) {
        item_array.sort(function(a, b) {
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
        item_array.reverse();
      }
    }
  }
}
</script>

<style>
  .el-table td .cell {
    white-space: pre-wrap;
  }
</style>
