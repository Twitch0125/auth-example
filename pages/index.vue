<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div>User: {{ $auth.user }}</div>
      <v-card v-if="!$auth.loggedIn">
        <v-card-title> Login </v-card-title>
        <v-card-text>
          <v-text-field v-model="email" label="email" />
          <v-text-field v-model="password" type="password" label="password" />
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" @click="login"> Login </v-btn>
          <v-btn @click="test"> test </v-btn>
        </v-card-actions>
      </v-card>
      <v-card v-else>
        <v-card-title>

        Welcome, {{$auth.user.username}}!
        </v-card-title>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  asyncData({$auth}){
    console.log('ssr', $auth.user)
  },
  data() {
    return {
      email: '',
      password: '',
    }
  },
  mounted(){
    console.log('client', this.$auth.user)
  },
  methods: {
    async login() {
      const data = {
        identifier: this.email,
        password: this.password,
      }
      const body = await this.$auth.loginWith('local', { data })
      console.log({body});
    },
    async test() {
      await this.$axios.$get('/api/pages')
    },
  },
}
</script>
