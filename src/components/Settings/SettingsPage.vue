<!--
  Copyright D3 Ledger, Inc. All Rights Reserved.
  SPDX-License-Identifier: Apache-2.0
-->
<template>
  <el-container class="fullheight">
    <el-main class="fullheight">
      <el-row class="container_flex fullheight">
        <SettingsLeftSide />
        <SettingsRightSide />
      </el-row>
    </el-main>
  </el-container>
</template>

<script>
import { lazyComponent } from '@router'
import { mapActions } from 'vuex'

export default {
  name: 'SettingsPage',
  components: {
    SettingsLeftSide: lazyComponent('Settings/SettingsLeftSide'),
    SettingsRightSide: lazyComponent('Settings/SettingsRightSide')
  },
  created () {
    this.updateAccount()
  },
  methods: {
    ...mapActions([
      'updateAccount'
    ])
  }
}
</script>

<style scoped>
.container_flex {
  display: flex;
  justify-content: space-between;
}

.left_column {
  margin-right: 0.5rem;
}

.right_column {
  display: flex;
  flex-direction: column;
}
</style>

<style scope>

</style>
