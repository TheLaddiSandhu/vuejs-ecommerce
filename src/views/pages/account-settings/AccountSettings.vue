<template>
  <v-card id="account-setting-card">
    <!-- tabs -->
    <v-tabs
      v-model="tab"
      show-arrows
    >
      <v-tab
        v-for="tab in tabs"
        :key="tab.icon"
      >
        <v-icon
          size="20"
          class="me-3"
        >
          {{ tab.icon }}
        </v-icon>
        <span>{{ tab.title }}</span>
      </v-tab>
    </v-tabs>

    <!-- tabs item -->
    <v-tabs-items v-model="tab">
      <!-- <v-tab-item>
        <account-settings-account :account-data="accountSettingData"></account-settings-account>
      </v-tab-item> -->

      <v-tab-item>
        <account-settings-security></account-settings-security>
      </v-tab-item>

      <!-- <v-tab-item>
        <account-settings-info :information-data="accountSettingData.information"></account-settings-info>
      </v-tab-item> -->
    </v-tabs-items>
  </v-card>
</template>

<script>
import { mdiAccountOutline, mdiLockOpenOutline, mdiInformationOutline } from '@mdi/js'
import { ref } from '@vue/composition-api'

// demos
import AccountSettingsAccount from './AccountSettingsAccount.vue'
import AccountSettingsSecurity from './AccountSettingsSecurity.vue'
import AccountSettingsInfo from './AccountSettingsInfo.vue'

export default {
  components: {
    AccountSettingsAccount,
    AccountSettingsSecurity,
    AccountSettingsInfo,
  },
  setup() {
    const tab = ref('')

    // tabs
    const tabs = [
      // { title: 'Account', icon: mdiAccountOutline },
      { title: 'Security', icon: mdiLockOpenOutline },
      // { title: 'Info', icon: mdiInformationOutline },
    ]

    return {
      tab,
      tabs,
      icons: {
        mdiAccountOutline,
        mdiLockOpenOutline,
        mdiInformationOutline,
      },
    }
  },
  computed: {
    accountSettingData() {
      return this.$store.state.users.filter((user) => user.id = this.$store.state.session.userId)[0];
    }
  }
}
</script>
