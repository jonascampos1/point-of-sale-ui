<template>
  <Login />
</template>

<script lang="ts">
import { defineComponent } from 'vue';

// Components
import Login from '../components/login.vue';

export default defineComponent({
  name: 'LoginView',

  components: {
    Login,
  },
});
</script>