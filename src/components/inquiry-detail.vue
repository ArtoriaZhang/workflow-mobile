<template>
  <div class="container-fluid">
    <div class="table">
      <div class="container-fluid">
        <div class="row"><div class="col-12 header">表单头信息</div></div>
        <div class="row">
          <div
            class="col"
            v-for="(item, index) in purOrderHeaderKeys"
            :key="index"
          >
            {{ purOrderHeaderKeyValue[item] }}
          </div>
        </div>
        <div class="row">
          <div
            class="col"
            v-for="(item, index) in purOrderHeaderKeys"
            :key="index"
          >
            <span :title="purOrderHeaderColumnValue[item]">{{
              purOrderHeaderColumnValue[item]
            }}</span>
          </div>
        </div>
      </div>
    </div>

    <div class="table">
      <GridAdayo
        :options="supInfoTableOptions"
        :columnData="supInfoTableColumnData"
        @rowClick="supHeaderRowClick"
        :selectedRow="this.selectedSup"
      ></GridAdayo>
    </div>

    <div class="table">
      <template v-if="this.selectedSup != null">
        <GridAdayo
          :options="inquiryInfoTableOptions"
          :columnData="inquiryInfoTableColumnData"
        ></GridAdayo>
      </template>
      <template v-else>
        <div class="row">
          <div class="col-12">请选择一个供应商查看报价详情</div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import GridAdayo from "./grid-template";

export default {
  components: {
    GridAdayo
  },
  data: function() {
    return {
      selectedSup: null
    };
  },
  props: {
    responseData: Object,
    purOrderHeaderKeys: {
      type: Array,
      default: () => {
        return ["orderNo", "type", "purUserName", "createDate"];
      }
    },
    purOrderHeaderKeyValue: {
      type: Object,
      default: () => {
        return {
          orderNo: "询价单号",
          type: "模板类型",
          purUserName: "制单人",
          createDate: "创建时间"
        };
      }
    },
    purOrderSupplierHeaderKeys: Array,
    inquiryDetailHeaderKeys: Array
  },
  computed: {
    purOrderHeaderColumnValue: function() {
      //console.log(this.responseData);
      return this.responseData.data ? this.responseData.data : {};
    },
    supInfoTableOptions: function() {
      // 拼接供应商表数据
      if (!this.responseData.data) {
        return {};
      }
      var options = {};
      options["tableName"] = "报价供应商";
      var headerKeysTmp = ["supCompanySrmCode", "supCompanyName"];
      options["headerKeys"] = headerKeysTmp;
      var headerKeyValueTmp = this.responseData.data.templateConf.orderItemPropertyDefList.filter(
        property =>
          headerKeysTmp.indexOf(property["code"]) >= 0 ? true : false
      );
      options["headerKeyValue"] = headerKeyValueTmp;
      //console.log(options);
      return options;
    },
    supInfoTableColumnData: function() {
      if (!this.responseData.data) {
        return [];
      }
      var columnDataTmp = [];
      columnDataTmp = this.responseData.data.inquirySuppliers.map(supplier => {
        return this.responseData.data.itemList.find(
          item => supplier.id === item.supTempCompanyId
        );
      });
      return columnDataTmp;
    },
    inquiryInfoTableOptions: function() {
      if (!this.responseData.data) {
        return;
      }
      var options = {};
      options["tableName"] = "报价详情";
      var headerKeysTmp = [
        "materialCode",
        "materialDesc",
        "untaxedUnitPrice",
        "targetPrice",
        "erpSystemPrice",
        "erpSystemPriceComparison"
      ];
      options["headerKeys"] = headerKeysTmp;
      var headerKeyValueTmp = this.responseData.data.templateConf.orderItemPropertyDefList.filter(
        property =>
          headerKeysTmp.indexOf(property["code"]) >= 0 ? true : false
      );
      options["headerKeyValue"] = headerKeyValueTmp;
      return options;
    },
    inquiryInfoTableColumnData: function() {
      if (!this.selectedSup) {
        return;
      }
      return this.responseData.data.itemList.filter(item => {
        return item.supTempCompanyId === this.selectedSup.supTempCompanyId;
      });
    }
  },
  methods: {
    supHeaderRowClick: function(supHeaderInfo) {
      console.log(supHeaderInfo);
      this.selectedSup = supHeaderInfo;
    },
    initTableOptions: function(tableName, headerKeys, responseData) {
      var options = {};
      options["tableName"] = "报价供应商";
      var headerKeysTmp = ["supCompanySrmCode", "supCompanyName"];
      options["headerKeys"] = headerKeysTmp;
      var headerKeyValueTmp = responseData.data.templateConf.orderItemPropertyDefList.filter(
        property =>
          headerKeysTmp.indexOf(property["code"]) >= 0 ? true : false
      );
      options["headerKeyValue"] = headerKeyValueTmp;
      return options;
    }
  },
  created: function() {
    console.log("supplier table info options", this.supInfoTableOptions);
  }
};
</script>

<style>
.header {
  text-align: center;
  margin-top: 5px;
}
</style>
