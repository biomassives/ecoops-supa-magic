
<template>
    <div class="col-6 form-widget mediacol flex flex-center">
      <center>
      <h3 class="header">Eco Ops App Profile Manger</h3>

      Sign in to update your profile via <BR/> magic link with your email below
      <BR />

      <img src="https://raw.githubusercontent.com/biomassives/ecoops-supa-magic/main/src/assets/eco_ops_giraffe_graphic2023-07-12-.avif" class="hero">

      </center>
    </div>
   



    <div class="col form-widget mediacol"  style="margin-top: 5rem">
      <div row>

      <b-card>
        <template #icon>
          <DocumentationIcon />
        </template>
        <template #heading></template>
        <b>Check ins</b><BR />
        Track activities that generate biodiversity credits & NFT tokens.
      </b-card>

      <p></p>

      <WelcomeItem>
        <template #icon>
          <CommunityIcon />
        </template>
        <template #heading></template>

        <b>Verification</b><BR />
        Community verifiers are experts in techniques for their regional
        context.
      </WelcomeItem>

  </div>
<div row>
   
 

      <WelcomeItem>
        <template #icon>
          <EcosystemIcon />
        </template>
        <template #heading></template>
        <b>Technique</b><BR />
        Biodiversity credits empower those effected by pollution & climate to restore
        ecosystems.
      </WelcomeItem>

      <p></p>
      <WelcomeItem>
        <template #icon>
          <SupportIcon />
        </template>
        <template #heading></template>
        <b>Transparency</b><BR />
        Participants provide checkin to show steps taken toward
        achieving project milestones.
      </WelcomeItem>


</div>

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

.mediacol {
  -webkit-columns: 2 200px;
  -moz-columns: 2 200px;
  columns: 2 200px;

}

@media (min-width: 500px) {
  .mediacol {
    height: 450px;
  }
}




  
</style>
