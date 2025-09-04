<template>
  <div class="auth-page">
    <div class="auth-container small">
      <div class="form-container compact">
        <div class="form-header">
          <h1 class="form-title">Forgot your password?</h1>
          <p class="form-subtitle">No worries! Enter your registered email or mobile number to reset your password.</p>
        </div>

        <form @submit.prevent="handleSend" class="auth-form" novalidate>
          <div class="form-group">
            <label for="contact" class="form-label">Email Address / Mobile Number</label>
            <div class="input-wrapper" :class="{ 'input-error': errors.contact }">
              <input
                id="contact"
                v-model="contact"
                type="text"
                class="form-input"
                placeholder="Enter your email or mobile number"
                @input="clearError('contact')"
                autocomplete="email"
              />
            </div>
            <div v-if="errors.contact" class="error-message" role="alert">{{ errors.contact }}</div>
          </div>

          <button type="submit" class="btn btn-primary" :disabled="isSending">
            <span v-if="!isSending">Send OTP / Reset Link</span>
            <span v-else>Sending...</span>
          </button>

          <div v-if="infoMessage" class="info-message">{{ infoMessage }}</div>
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
  name: 'ForgotPassword',
  data() {
    return {
      contact: '',
      errors: {},
      isSending: false,
      infoMessage: ''
    }
  },
  methods: {
    isValidEmail(value) {
      const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
      return re.test(value)
    },
    isValidMobile(value) {
      const re = /^[0-9]{6,15}$/
      return re.test(value)
    },
    clearError(field) {
      if (this.errors[field]) delete this.errors[field]
      this.infoMessage = ''
    },
    async handleSend() {
      this.errors = {}
      if (!this.contact) {
        this.errors.contact = 'Please provide your email or mobile number.'
        return
      }

      const via = this.isValidEmail(this.contact) ? 'email' : (this.isValidMobile(this.contact) ? 'mobile' : null)
      if (!via) {
        this.errors.contact = 'Please enter a valid email or mobile number.'
        return
      }

      this.isSending = true
      try {
        // Simulate API call
        await new Promise(r => setTimeout(r, 900))

        // Set message and navigate to verification step
        this.infoMessage = `We've sent a verification ${via === 'mobile' ? 'code' : 'link'} to ${this.contact}. Please check and enter below.`

        // After a short delay, navigate to verification step
        setTimeout(() => {
          this.$router.push({ name: 'VerifyReset', query: { via, contact: this.contact } })
        }, 800)
      } catch (err) {
        this.errors.contact = 'Unable to send reset instructions. Please try again later.'
      } finally {
        this.isSending = false
      }
    }
  }
}
</script>

<style scoped>
.form-container.compact {
  max-width: 520px;
  margin: 4rem auto;
  padding: 2rem;
  background: white;
  border-radius: 8px;
  box-shadow: var(--shadow-sm);
}
.form-title { font-size: 1.5rem; margin-bottom: 0.5rem }
.form-subtitle { color: var(--text-secondary); margin-bottom: 1rem }
.info-message { margin-top: 1rem; color: var(--text-secondary) }
</style>
