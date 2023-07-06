
<template>
  <div class="form-widget">
    <h1 class="header">Supabase + Vue.js: Account</h1>
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
