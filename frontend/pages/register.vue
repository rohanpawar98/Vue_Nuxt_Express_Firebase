<template>
  <v-container fill-height>
    <v-row justify="center" align="center">
      <v-col cols="12" md="4">
        <div class="text-h4 text-center">Register</div>

        <v-divider class="mt-5 mb-5" />
        <v-form
          ref="form"
          justify="center"
          align="center"
          @submit.prevent="addUser({ email, password })"
        >
          <v-text-field
            v-model="email"
            label="Email"
            placeholder="Enter your email"
            type="email"
            :rules="emailRules"
            autofocus
            shaped
            outlined
            class="mt-2 mb-2"
          >
          </v-text-field>
          <v-text-field
            v-model="password"
            label="Password"
            placeholder="Enter your password"
            type="password"
            :rules="passwordRules"
            shaped
            outlined
            class="mt-2 mb-2"
          >
          </v-text-field>
          <v-btn class="mt-2 mb-2" type="submit">Register</v-btn>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapActions } from 'vuex'
export default {
  auth: false,
  data: () => ({
    email: '',
    password: '',
    emailRules: [
      (v) => !!v || 'E-mail is required',
      (v) => /.+@.+\..+/.test(v) || 'E-mail must be valid',
    ],
    passwordRules: [
      (v) => !!v || 'Password is required',
      (v) => (v && v.length >= 8) || 'Password must be more than 8 characters',
    ],
  }),
  head: {
    title: 'Register',
  },
  methods: {
    ...mapActions('user', ['register']),
    addUser({ email, password }) {
      try {
        this.register({ email, password })
          .then((res) => {
            if (res.data.error) {
              console.log(res.data.error)
              this.$toast.error(`😔 ${res.data.error.message}`).goAway(3000)
            } else {
              this.$toast.success(`🏆${res.data.message}`).goAway(3000)
            }
          })
          .then(() => this.$refs.form.reset())
      } catch (error) {}
    },
  },
}
</script>
<style scoped></style>
