<template>
  <div class="auth-page">
    <div class="auth-container small">
      <div class="form-container compact">
        <div class="form-header">
          <h1 class="form-title">Reset Password</h1>
          <p class="form-subtitle">Set a new password for your account.</p>
        </div>

        <form @submit.prevent="handleSave" class="auth-form">
          <div class="form-group">
            <label class="form-label">New Password</label>
            <div class="input-wrapper">
              <input :type="showPassword ? 'text' : 'password'" v-model="password" class="form-input" placeholder="New password" />
              <button type="button" class="password-toggle" @click="showPassword = !showPassword">{{ showPassword ? 'Hide' : 'Show' }}</button>
            </div>
          </div>

          <div class="form-group">
            <label class="form-label">Confirm Password</label>
            <input type="password" v-model="confirmPassword" class="form-input" placeholder="Confirm password" />
          </div>

          <button class="btn btn-primary" type="submit" :disabled="isSaving">Save Password</button>

          <div v-if="successMessage" class="success-message">{{ successMessage }}</div>
          <div v-if="errors.general" class="error-message">{{ errors.general }}</div>
        </form>

        <div class="auth-switch">
          <router-link to="/login" class="switch-link">Back to Sign in</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ResetPassword',
  data() {
    return {
      password: '',
      confirmPassword: '',
      showPassword: false,
      isSaving: false,
      successMessage: '',
      errors: {}
    }
  },
  methods: {
    validate() {
      this.errors = {}
      if (!this.password || this.password.length < 6) {
        this.errors.general = 'Password must be at least 6 characters.'
        return false
      }
      if (this.password !== this.confirmPassword) {
        this.errors.general = 'Passwords do not match.'
        return false
      }
      return true
    },
    async handleSave() {
      if (!this.validate()) return
      this.isSaving = true
      try {
        await new Promise(r => setTimeout(r, 800))
        this.successMessage = 'Your password has been reset successfully. You can now sign in with your new password.'
        setTimeout(() => {
          this.$router.push('/login')
        }, 1200)
      } catch (err) {
        this.errors.general = 'Unable to reset password. Please try again.'
      } finally {
        this.isSaving = false
      }
    }
  }
}
</script>

<style scoped>
.form-container.compact { max-width: 520px; margin: 4rem auto; padding: 2rem; background: white; border-radius: 8px; box-shadow: var(--shadow-sm); }
.password-toggle { margin-left: 0.5rem; }
.success-message { color: #10b981; margin-top: 1rem }
</style>
