<template>
  <layout>
    <h3 class="title">{{ titles.emailForgotTitle }}</h3>
    <div v-if="message.sumary" :class="`alert-${message.type}`">
      <span v-if="message.type" v-html="getIcon(message.type)"></span>
      <span v-if="message.sumary">{{ message.sumary }}</span>
    </div>
    <form :action="getUrl(urls.loginAction)" method="post">
      <div
        :class="
          validations.usernameOrPassword ? 'form-group error' : 'form-group'
        "
      >
        <label for="username">{{ getUsernameLabel() }}</label>
        <input
          tabindex="1"
          name="username"
          :value="forms.loginUsername"
          type="text"
        />
        <span v-if="validations.usernameOrPassword">{{ validations.usernameOrPassword }}</span>
      </div>
      <button tabindex="4" type="submit">{{ labels.doSubmit }}</button>
    </form>
    <div class="login">
      <a :href="getUrl(urls.login)">{{ labels.backToLogin }}</a>
    </div>
  </layout>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import Layout from '~/components/Layout.vue'
import { useLogin } from '~/hooks'

export default defineComponent({
  name: 'ResetPassword',
  components: {
    Layout
  },
  setup() {
    return useLogin()
  },
  mounted() {
    console.log('ResetPassword')
  }
})
</script>
