<!--
  Copyright D3 Ledger, Inc. All Rights Reserved.
  SPDX-License-Identifier: Apache-2.0
-->
<template>
  <el-table
    ref="table"
    :data="settlements"
    class="settlements_table"
    @row-dblclick="(row) => this.$refs.table.toggleRowExpansion(row)"
  >
    <el-table-column type="expand">
      <template slot-scope="scope">
        <div class="transaction_details">
          <el-row>
            <el-col :span="6">{{ formatDateLong(scope.row.from.date) }}</el-col>
            <el-col
              :span="6"
              class="transaction_details-amount"
            >
              <p>- {{ scope.row.from.amount }} {{ assetName(scope.row.from.assetId) }}</p>
              <p>+ {{ scope.row.to.amount }} {{ assetName(scope.row.to.assetId) }}</p>
            </el-col>
            <el-col :span="6">{{ scope.row.from.message }}</el-col>
            <el-col :span="6">{{ scope.row.from.to }}</el-col>
          </el-row>
        </div>
      </template>
    </el-table-column>
    <el-table-column
      label="Amount"
      min-width="200"
    >
      <template slot-scope="scope">
        {{ scope.row.from.amount }} {{ assetName(scope.row.from.assetId) }}
        {{ '→' }}
        {{ scope.row.to.amount }} {{ assetName(scope.row.to.assetId) }}
      </template>
    </el-table-column>
    <el-table-column
      label="Counterparty"
      min-width="120"
    >
      <template slot-scope="scope">
        <div>
          {{ scope.row.from.to }}
        </div>
      </template>
    </el-table-column>
    <el-table-column
      label="Date"
      width="120"
    >
      <template slot-scope="scope">
        {{ formatDate(scope.row.from.date) }}
      </template>
    </el-table-column>
    <el-table-column width="200"/>
  </el-table>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import dateFormat from '@/components/mixins/dateFormat'
import currencySymbol from '@/components/mixins/currencySymbol'

export default {
  mixins: [
    dateFormat,
    currencySymbol
  ],
  data () {
    return {}
  },
  computed: {
    ...mapGetters({
      settlements: 'resolvedSettlements',
      wallets: 'wallets'
    })
  },
  created () {
    Promise.all([
      this.getAccountAssets(),
      this.getAllAssetsTransactions()
    ])
  },
  methods: {
    ...mapActions([
      'getAccountAssets',
      'getAllAssetsTransactions'
    ])
  }
}
</script>

<style scoped>
.settlements_table >>> .el-table__header th {
  font-weight: 500;
}
.settlements_table >>> .el-table__row td .cell {
  color: #000000;
}
.settlements_table >>> .el-table__expanded-cell {
  padding: 0rem 1rem 1rem;
}
.transaction_details {
  font-size: 0.8rem;
  color: #000000;
  background-color: #f4f4f4;
  padding: 1rem;
}
.transaction_details-amount {
  flex-wrap: wrap;
  font-weight: 600;
}
.status-tag {
  text-transform: capitalize;
}
</style>
