<template>
  <layout>
    <h3 class="title">{{ titles.loginAccountTitle }}</h3>
    <div v-if="message.sumary" :class="`alert-${message.type}`">
      <span v-html="getIcon(message.type)"></span>
      <span>{{ message.sumary }}</span>
    </div>
    <form :action="getUrl(urls.loginAction)" method="post">
      <div v-if="social && social.length > 0" class="social">
        <a
        v-for="item in social"
        :key="item.alias"
        style="width: 347px; height: 58px; border-radius: 39px;"
        :href="getUrl(item.loginUrl)">
          <span v-html="getIcon(item.alias)"></span>
          <span>Login with {{ item.displayName }}</span>
        </a>
      </div>
      <div class="logo-container" style="margin-top: 20px;">
        <span>powered by</span>
        <span v-html="getIcon('comparaja')"></span>
      </div>
      <!-- TODO - /** In the future, please remove the comments below so we can display more information */ -->
      <!-- <span
        v-html="getIcon('arrow')"
        class="arrow"
        :class="{ 'rotate': showCompleteForm || validations.usernameOrPassword }"
        @click="showCompleteForm = !showCompleteForm">
      </span> -->
      <div
      v-if="showCompleteForm || validations.usernameOrPassword"
        :class="
          validations.usernameOrPassword && isFormIncomplete ? 'form-group error' : 'form-group'
        "
      >
        <label for="username">{{ getUsernameLabel() }}</label>
        <input
          tabindex="1"
          name="username"
          v-model="loginUsername"
          type="text"
        />
        <span
          v-if="validations.usernameOrPassword && isFormIncomplete"
          class="error-message">
            {{ validations.usernameOrPassword }}
        </span>
      </div>
      <div
        v-if="showCompleteForm || validations.usernameOrPassword"
        :class="
          validations.usernameOrPassword && isFormIncomplete ? 'form-group error' : 'form-group'
        "
      >
        <label style="margin-top: 14px;" for="password">{{ labels.password }}</label>
        <input
          tabindex="2"
          name="password"
          type="password"
          v-model="loginPassword"
          autocomplete="off"
        />
        <span
          v-if="validations.usernameOrPassword && isFormIncomplete"
          class="error-message">
            {{ validations.usernameOrPassword }}
        </span>
      </div>

      <div
        v-if="permissions.rememberMe && !permissions.usernameEditDisabled && showCompleteForm || validations.usernameOrPassword"
        class="checkbox"
        style="margin-bottom: 28px;"
      >
        <label>
          <input
            tabindex="3"
            name="rememberMe"
            type="checkbox"
            :checked="forms.loginRememberMe"
          />
          <span v-if="labels.rememberMe" class="remember-me">{{ labels.rememberMe }}</span>
        </label>
      </div>
      <input
        v-if="showCompleteForm || validations.usernameOrPassword"
        type="hidden"
        name="credentialId"
        :value="forms.selectedCredential"
      />
      <button
      v-if="showCompleteForm || validations.usernameOrPassword"
      tabindex="5"
      type="submit"
      :disabled="isFormIncomplete">
        {{ labels.doLogIn }}
      </button>
      <span v-if="permissions.resetPasswordAllowed && showCompleteForm || validations.usernameOrPassword">
        <a
          tabindex="4"
          :href="getUrl(urls.loginResetCredentials)"
          class="forgot-password"
          >{{ labels.doForgotPassword }}</a
        >
      </span>
    </form>
    <div
      class="register"
      v-if="
        permissions.password &&
        permissions.registrationAllowed &&
        !permissions.registrationDisabled
      "
    >
      <span v-if="labels.noAccount">{{ labels.noAccount }}</span>
      <a :href="getUrl(urls.registration)">{{ labels.doRegister }}</a>
    </div>
  </layout>
</template>
<script lang="ts">
import { defineComponent, ref, computed } from 'vue'
import Layout from '~/components/Layout.vue'
import { useLogin } from '~/hooks'

export default defineComponent({
  name: 'Login',
  components: {
    Layout
  },
  setup() {
    const { forms } = useLogin()
    const loginForms = forms.value

    const loginUsername = ref(loginForms.loginUsername)
    const loginPassword = ref('')
    const showCompleteForm = ref(false)

    const isFormIncomplete = computed(() => {
      return !loginUsername.value || !loginPassword.value
    })

    return {
      ...useLogin(),
      loginUsername,
      loginPassword,
      showCompleteForm,
      isFormIncomplete
    }
  }
})
</script>
