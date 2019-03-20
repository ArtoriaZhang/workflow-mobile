<template>
  <div class="container-fluid">
    <template v-if="tableName">
      <div class="row header">
        <div class="col-12">{{ tableName }}</div>
      </div>
    </template>

    <template v-if="Array.isArray(headerKeys)">
      <div class="row">
        <div class="col" v-for="(value, index) in headerKeys" :key="index">
          <span v-for="(entry, i) in headerKeyValue" :key="i">
            {{ entry["code"] === value ? entry["displayName"] : "" }}
          </span>
        </div>
      </div>
    </template>

    <template v-else>
      <tr class="row">
        <th
          v-for="(value, index) in headerKeys"
          :key="index"
          @click="sortBy(value);"
          class="col"
          :class="{ active: sortKey == value }"
        >
          {{ headerKeyValue[value] }}
          <span class="arrow" :class="sortOrders[value] > 0 ? 'asc' : 'dsc'">
          </span>
        </th>
      </tr>
    </template>

    <div
      class="row"
      v-for="(entry, index) in columnData"
      :key="index"
      @click="$emit('rowClick', entry);"
    >
      <div
        class="col"
        :class="{ selected: selectedRow === entry }"
        v-for="(key, indexx) in headerKeys"
        :key="indexx"
      >
        <span :title="entry[key]">{{ entry[key] }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    options: Object,
    columnData: Array,
    filterKey: String,
    columns: Array,
    selectedRow: null
  },
  data: function() {
    var sortOrders = {};
    this.columns.forEach(function(key) {
      sortOrders[key] = 1;
    });
    return {
      sortKey: "",
      sortOrders: sortOrders
    };
  },
  computed: {
    tableName: function() {
      return this.options.tableName ? this.options.tableName : "";
    },
    headerKeys: function() {
      return this.options.headerKeys ? this.options.headerKeys : [];
    },
    headerKeyValue: function() {
      return this.options.headerKeyValue ? this.options.headerKeyValue : [];
    },
    filteredHeroes: function() {
      var sortKey = this.sortKey;
      var filterKey = this.filterKey && this.filterKey.toLowerCase();
      var order = this.sortOrders[sortKey] || 1;
      var heroes = this.heroes;
      if (filterKey) {
        heroes = heroes.filter(function(row) {
          return Object.keys(row).some(function(key) {
            return (
              String(row[key])
                .toLowerCase()
                .indexOf(filterKey) > -1
            );
          });
        });
      }
      if (sortKey) {
        heroes = heroes.slice().sort(function(a, b) {
          a = a[sortKey];
          b = b[sortKey];
          return (a === b ? 0 : a > b ? 1 : -1) * order;
        });
      }
      return heroes;
    }
  },
  filters: {
    capitalize: function(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  },
  methods: {
    sortBy: function(key) {
      this.sortKey = key;
      this.sortOrders[key] = this.sortOrders[key] * -1;
    }
  },
  created: function() {
    this.headerKeys.forEach(item => {
      this.headerKeyValue.forEach(entry => {
        //console.log(entry["code"] === item ? entry["displayName"] : null);
      });
    });
    // console.log(this.headerKeyValue);
    // console.log("grid column data:", this.columnData);
  }
};
</script>

<style>
.selected {
  background-color: #84bbdb;
}
</style>
