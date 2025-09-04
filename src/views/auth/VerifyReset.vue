<template>
  <div class="auth-page">
    <div class="auth-container small">
      <div class="form-container compact">
        <div class="form-header">
          <h1 class="form-title">Verification</h1>
          <p class="form-subtitle">We've sent a verification {{ via === 'mobile' ? 'code' : 'link' }} to {{ contact }}. Please check and follow the instructions below.</p>
        </div>

        <div v-if="via === 'mobile'">
          <form @submit.prevent="handleVerifyOtp" class="auth-form">
            <div class="form-group">
              <label class="form-label">Enter OTP</label>
              <input v-model="otp" type="text" class="form-input" placeholder="Enter OTP" />
            </div>
            <button type="submit" class="btn btn-primary" :disabled="isVerifying">Verify & Continue</button>
          </form>
        </div>

        <div v-else>
          <p class="info">Open the link from your email and click the reset link. If you already received it, click Continue to proceed.</p>
          <button class="btn btn-primary" @click="goToReset" :disabled="isVerifying">I have the link — Continue</button>
        </div>

        <div v-if="message" class="info-message">{{ message }}</div>

        <div class="auth-switch">
          <router-link to="/login" class="switch-link">Back to Sign in</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VerifyReset',
  data() {
    return {
      via: this.$route.query.via || 'email',
      contact: this.$route.query.contact || '',
      otp: '',
      isVerifying: false,
      message: ''
    }
  },
  methods: {
    async handleVerifyOtp() {
      if (!this.otp || this.otp.trim().length < 3) {
        this.message = 'Please enter the verification code.'
        return
      }
      this.isVerifying = true
      try {
        await new Promise(r => setTimeout(r, 800))
        // Simulate verification success
        this.$router.push({ name: 'ResetPassword' })
      } catch (err) {
        this.message = 'Verification failed. Please try again.'
      } finally {
        this.isVerifying = false
      }
    },
    goToReset() {
      this.$router.push({ name: 'ResetPassword' })
    }
  }
}
</script>

<style scoped>
.form-container.compact { max-width: 520px; margin: 4rem auto; padding: 2rem; background: white; border-radius: 8px; box-shadow: var(--shadow-sm); }
.info { color: var(--text-secondary); margin-bottom: 1rem }
.info-message { margin-top: 1rem; color: var(--text-secondary) }
</style>
