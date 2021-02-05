<template>
  <v-container>
    <h1 class="font-weight-light text-center">
      <v-icon color="deep-orange" x-large>mdi-flag-outline</v-icon>
      Report Content
    </h1>
    <v-divider></v-divider>
    <div v-if="reportContent">
      <v-row justify="space-around">
        <v-col cols="12" md="4">
          <v-text-field
              v-model="postUsername"
              outlined
              dense
              flat
              disabled
              class="font-weight-light mt-3"
              prefix="@"
              color="deep-orange"
              label="Username"
              type="text"
          />
          <v-text-field
              v-model="postId"
              outlined
              dense
              flat
              disabled
              class="font-weight-light"
              prefix="@"
              color="deep-orange"
              label="Post ID"
              type="text"
          />

          <v-radio-group v-model="selectedType" dense mandatory>
            <v-radio
                v-for="report in reportTypes"
                color="deep-orange"
                :key="report.type"
                :label="report.type"
                :value="report.type"
            ></v-radio>
          </v-radio-group>

          <div class="font-weight-light mb-3">If you have additional information, you can write below</div>

          <v-text-field
              v-model="reportComment"
              outlined
              dense
              flat
              class="font-weight-light"
              color="deep-orange"
              label="Information"
              type="text"
          />

          <v-btn block color="red" dark @click="report">
            <v-icon color="white" left>mdi-flag-outline</v-icon>
            Report
          </v-btn>
        </v-col>
      </v-row>
    </div>
    <div v-else class="font-weight-light text-h5 text-center mt-3">
      Please use the report button from the context menu of the post you want to report. If you need help, visit our
      <router-link to="/help" class="deep-orange--text">Help</router-link>
      page or
      <router-link to="/contact" class="deep-orange--text">contact</router-link>
      with us.
    </div>

    <v-dialog v-model="reportDialog" width="400">
      <v-card>
        <v-card-title class="font-weight-light">
          {{ reportOk ? 'Reported' : 'Report failed' }}
        </v-card-title>

        <v-divider></v-divider>

        <v-card-text class="mt-3">
          {{ reportOk ? 'Thank you for making Vevericka a better place' : 'Please try again later' }}
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="success" text @click="completeReport">
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import { router } from '@/router';

export default {
  name: "ReportPage",
  props: [],
  data: () => ({
    loading: true,
    reportTypes: [],
    postId: '',
    postUsername: '',
    selectedType: '',
    reportComment: '',
    reportOk: false,
    reportDialog: false,
  }),
  mounted() {
    this.postId = this.$route.params.postId;
    this.postUsername = this.$route.params.postUsername;
    this.fetchReportTypes().then(() => {
      this.selectedType = this.reportTypes[0];
      this.loading = false;
    })
  },
  methods: {
    async fetchReportTypes() {
      const URL = "https://vevericka-report-service.herokuapp.com/api/v1/report_types";
      const response = await fetch(URL);
      const {data} = await response.json();
      this.reportTypes = data;
    },
    async report() {
      const URL = "https://vevericka-report-service.herokuapp.com/api/v1/";
      const requestBody = {
        reported_post_id: this.postId,
        reported_user: this.postUsername,
        reported_by: this.$store.state.user.username,
        reported_by_comment: this.reportComment,
        type: this.selectedType,
      };

      const requestOptions = {
        method: "POST",
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify(requestBody),
      };

      const response = await fetch(URL, requestOptions);

      this.reportOk = response.status === 201;
      this.reportDialog = true;
    },
    completeReport() {
      this.reportDialog = false;
      router.push('/');
    }
  },
  computed: {
    reportContent() {
      return this.postId !== undefined
          && this.postUsername !== undefined
          && this.postId !== ''
          && this.postUsername !== '';
    }
  }
}
</script>

<style scoped>

</style>