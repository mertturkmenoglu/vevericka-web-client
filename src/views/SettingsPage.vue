<template>
  <v-container>
    <v-card flat>
      <v-toolbar flat color="deep-orange" dark dense>
        <v-toolbar-title class="font-weight-light">{{ $t('settings.card_title') }}</v-toolbar-title>
      </v-toolbar>
      <SettingsTabs v-if="loadingCompleted" :user="user"/>
    </v-card>
  </v-container>
</template>

<script lang="ts">
import Vue from "vue";
import SettingsTabs from "@/components/Settings/SettingsTabs.vue";
import {Component} from "vue-property-decorator";
// eslint-disable-next-line no-unused-vars
import {IUser} from "@/api/responses/IUser";
import UserInfoService from "@/api/UserInfoService";

@Component({
  name: "SettingsPage",
  components: {SettingsTabs}
})
export default class SettingsPage extends Vue {
  user: IUser = ({} as IUser)
  loadingCompleted: boolean = false

  mounted() {
    this.fetchUser().then(() => this.loadingCompleted = true)
  }

  async fetchUser() {
    const username = this.$store.state.user.username;
    const [result, err] = await UserInfoService.getUserByUsername(username)

    if (err === null && result !== null) {
      this.user = result
    }
  }
}
</script>

<style>
</style>
