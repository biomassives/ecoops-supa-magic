
<template>
  <div class="row flex flex-center">
    <div class="col-6 form-widget">
      
      <img src="https://raw.githubusercontent.com/biomassives/ecoops-supa-magic/main/src/assets/eco_ops_giraffe_graphic2023-07-12-.avif" class="hero">
      <h1 class="header">Eco Ops App</h1>
      <p class="description">Sign in to update your profile via magic link with your email below</p>
      <div>
        <input
          class="inputField"
          type="email"
          placeholder="Your email"
          v-model="email"
        />
      </div>
      <div>
        <button
          @click="
            e => {
              e.preventDefault();
              handleLogin(email);
            }
          "
          class="button block"
          :disabled="loading"
        >
          <span>{{ loading ? "Loading..." : "Send Magic Link" }}</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import { supabase } from "./supabaseClient";

export default defineComponent({
  name: "Auth",
  setup() {
    const loading = ref(false);
    const email = ref("");

    const handleLogin = async email => {
      try {
        loading.value = true;
        const { error } = await supabase.auth.signInWithOtp({ email });
        if (error) throw error;
        alert("Check your email for the login link!");
      } catch (error) {
        alert(error.error_description || error.message);
      } finally {
        loading.value = false;
      }
    };

    return {
      email,
      loading,
      handleLogin
    };
  }
});
</script>

<style scoped>
</style>
