<template>
  <v-card flat>
    <v-card-title class="card-title">{{ $t('user.wish_to_speak.title') }}</v-card-title>
    <v-simple-table v-if="user.wish_to_speak.length > 0">
      <template v-slot:default>
        <thead>
        <tr>
          <th class="text-left">{{ $t('user.wish_to_speak.language') }}</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(l, idx) in user.wish_to_speak" :key="idx">
          <td>{{ l }}</td>
        </tr>
        </tbody>
      </template>
    </v-simple-table>
    <div v-else class="text-h6 text-center font-weight-light mt-3">{{ $t('user.wish_to_speak.no_languages') }}</div>
  </v-card>
</template>

<script>
  export default {
    name: "UserWishToSpeak",
    props: ["user"],
  }
</script>

<style scoped>
  .card-title {
    color: white;
    background-color: #E64A19;
  }
</style>
