<template>
  <v-container class="fill-height" fluid>
    <v-row align="center" justify="center" no-gutters>
      <v-col cols="12">
        <v-card class="elevation-12 card">
          <v-row class="fill-height">
            <v-col cols="12" md="8" class="darken-2 vcenter" :class="`${bgColor}`">
              <div>
                <div class="text-center mb-6">
                  <v-img class="mx-auto" max-height="256" max-width="256" src="../assets/icon_white.svg"/>
                </div>
                <v-card-text :class="`${fgColor}--text`">
                  <h1 class="text-center text-h2 headline mb-3">Vevericka</h1>
                  <h5 class="text-center overline mb-3">Wingardium Leviosa</h5>
                </v-card-text>
              </div>
            </v-col>
            <v-col cols="12" md="4" class="pt-6 pb-6 vcenter">
              <v-card-text>
                <h1 class="text-center display-1 mb-10 deep-orange--text text--darken-2" >
                  Forgot Password
                </h1>
                <v-form class="reset-form-form" @submit.prevent="submit">
                  <div class="text-center mt-5 text-body-1">
                    <span class="grey--text text--darken-1">Enter the email address you used to register with. </span>
                    <span class="grey--text text--darken-1">We will send you a password reset email.</span>
                  </div>
                  <v-text-field
                      class="pt-5"
                      label="Email"
                      v-model="email"
                      color="deep-orange text--darken-2"
                      outlined
                      dense
                  />
                  <div class="text-center mt-3">
                    <v-btn @click="submit" color="deep-orange text--darken-2" outlined block>Send Reset Code</v-btn>
                  </div>
                  <div class="text-center mt-5 text-body-1">
                    <span class="grey--text text--darken-1">Have an account? </span>
                    <router-link to="/login" class="link">Login</router-link>
                  </div>
                  <div class="text-center mt-5 text-body-1">
                    <span class="grey--text text--darken-1">New user? </span>
                    <router-link to="/register" class="link">Register</router-link>
                  </div>
                  <v-snackbar v-model="snackbar" absolute right>
                    {{ snackbarMessage }}
                    <template v-slot:action="{ attrs }">
                      <v-btn color="pink" text v-bind="attrs" @click="snackbar = false">
                        Close
                      </v-btn>
                    </template>
                  </v-snackbar>
                </v-form>
              </v-card-text>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: "PasswordResetPage",
    data: () => ({
      email: "",
      bgColor: 'deep-orange darken-2',
      fgColor: 'white',
      snackbar: false,
      snackbarMessage: "Currently unavailable",
    }),
    methods: {
      submit() {
        this.snackbar = true;
      },
    },
  }
</script>

<style scoped lang="scss">
  a.no-text-decoration {
    text-decoration: none;
  }

  .reset-form-form {
    max-width: 25rem;
    margin: 0 auto;
  }

  .card {
    overflow-x: hidden;
    overflow-y: auto;
    height: 97vh;
  }

  .vcenter {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .link {
    color: #E64A19;
  }
</style>
