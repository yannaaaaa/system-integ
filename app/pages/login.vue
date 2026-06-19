<template>
  <div class="d-flex mx-auto justify-center align-center" style="height: 95vh;">
    <v-card class="pa-8 text-center" width="500" elevation="4" rounded="x1">
      <p class="text-center text-headline-large font-weight-bold">Login</p>
      <p class="text-center">Welcome back, please login</p>

      <v-form>
        <v-text-field label="Email" variant="solo-filled" prepend-inner-icon="mdi-email" flat></v-text-field>
        <v-text-field label="Password" variant="solo-filled" prepend-inner-icon="mdi-lock" flat></v-text-field>
      </v-form>

      <v-btn block variant="tonal" color="primary">Sign In</v-btn>

      <div class="mt-8">OR</div>

      <v-btn 
        class="my-8" 
        block variant = "elevated"
        color="red" prepend-icon="mdi-google"
        @click="loginWithGoogle"> Sign in with Google</v-btn>
    </v-card>

  </div>

</template>

<script lang ="ts" setup> 

//@ts-nocheck
const config = useRuntimeConfig()

declare global {
  interface Window {
    google: any
  }
}

const loginWithGoogle = () => {
  const client = window.google.accounts.oauth2.initTokenClient({
    client_id: config.public.googleClientId,
    scope: 'openid email profile',
    callback: async (response: any) => {
      const userInfo = await $fetch('http://www.googleapis.com/oauth2/v3/userinfo',{
        headers: {
          Authorization: `Bearer ${response.access_token}`
        }
      })
      localStorage.setItem('google_user', JSON.stringify(userInfo))
      localStorage.setItem('google_token', response.access_token)

      navigateTo('/')
    }
  })
  client.requestAccessToken()

}
</script>