
<template>

<div>
    <tabs :options="{ useUrlFragment: false }" @clicked="tabClicked" @changed="tabChanged" nav-item-class="nav-item">
        <tab name="First tab">
            This is the content of the first tab
        </tab>
        <tab name="Second tab">
            This is the content of the second tab
        </tab>
        <tab name="Disabled tab" :is-disabled="true">
            This content will be unavailable while :is-disabled prop set to true
        </tab>
        <tab id="oh-hi-mark" name="Custom fragment">
            The fragment that is appended to the url can be customized
        </tab>
        <tab prefix="<span class='glyphicon glyphicon-star'></span> " 
             name="Prefix and suffix" 
             suffix=" <span class='badge'>4</span>">
            A prefix and a suffix can be added
        </tab>
    </tabs>
</div>


  <div class="form-widget">
    <h1 class="header">Eco Ops App Profile</h1>
    <avatar :url="avatar_url" @onUpload="handleImageUpload" />
    <div>
      <label htmlFor="email">Email</label>
      <input id="email" type="text" :value="session.user.email" disabled />
    </div>
    <div>
      <label htmlFor="username">Name</label>
      <input id="username" type="text" v-model="username" />
    </div>
    <div>
      <label htmlFor="website">Website</label>
      <input id="website" type="website" v-model="website" />
    </div>
    <div>
      <label htmlFor="group">Group</label>
      <input id="group" type="group" v-model="group" />
    </div>
    <div>
      <label htmlFor="project">Project</label>
      <input id="project" type="project" v-model="project" />
    </div>

    <div>
      <button
        class="button block primary"
        @click="updateProfile()"
        :disabled="loading"
      >
        <span>{{ loading ? "Loading..." : "Update" }}</span>
      </button>
    </div>

    <div>
      <button class="button block" @click="supabase.auth.signOut()">
        Sign Out
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, nextTick, ref } from "vue";
import Avatar from "./Avatar.vue";
import { supabase } from "./supabaseClient";


import {Tabs, Tab} from 'vue3-tabs-component';



export default defineComponent({
  name: "Account",
  props: ["session"],
  components: {
    Avatar
  },
  setup(props) {
    const loading = ref(false);
    const username = ref("");
    const website = ref("");
    const group = ref("");
    const project = ref("");
    const avatar_url = ref("");

    /**
     *
     */
    const handleImageUpload = async path => {
      avatar_url.value = path;
      await updateProfile();
    };

    const updateProfile = async () => {
      try {
        debugger;

        await nextTick();

        loading.value = true;
        const {data: { user } , error: userError } = await supabase.auth.getUser();
        if ( userError) throw userError

        const updates = {
          id: user?.id,
          username : username.value,
          website: website.value,         
          group: group.value,
          project: project.value,
          avatar_url: (avatar_url.value || avatar_url),
          updated_at: new Date()
        };

        let { error } = await supabase.from("profiles").upsert(updates);

        if (error) {
          throw error;
        }
      } catch (error) {
        alert(error.message);
      } finally {
        loading.value = false;
      }
    };

    const getProfile = async session => {
      try {
        loading.value = true;
        const user = session.user;

        let { data, error, status } = await supabase
          .from("profiles")
          .select(`username, website, project, group, avatar_url`)
          .eq("id", user.id)
          .single();

        if (error && status !== 406) {
          throw error;
        }

        if (data) {
          username.value = data.username;
          website.value = data.website;
          group.value = data.group;
          project.value = data.project;
          avatar_url.value = data.avatar_url;
        }

        debugger;
      } catch (error) {
        alert(error.message);
      } finally {
        loading.value = false;
      }
    };

    getProfile(props.session);

    return {
      loading,
      username,
      website,
      group,
      project,
      avatar_url,
      updateProfile,
      supabase,
      handleImageUpload
    };
  }
});
</script>

<style scoped>
</style>
