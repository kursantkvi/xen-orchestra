<template>
  <div class="app-login form-container">
    <form @submit.prevent="handleSubmit">
      <img alt="XO Lite" src="../assets/logo-title.svg" />
      <input v-model="login" name="login" readonly type="text" />
      <input
        v-model="password"
        :readonly="isConnecting"
        name="password"
        :placeholder="$t('password')"
        type="password"
      />
      <UiButton :busy="isConnecting" type="submit">
        {{ $t("login") }}
      </UiButton>
    </form>
  </div>
</template>

<script lang="ts" setup>
import { storeToRefs } from "pinia";
import { onMounted, ref } from "vue";
import UiButton from "@/components/ui/UiButton.vue";
import { useXenApiStore } from "@/stores/xen-api.store";

const xenApiStore = useXenApiStore();
const { isConnecting } = storeToRefs(xenApiStore);
const login = ref("root");
const password = ref("");

onMounted(() => {
  xenApiStore.reconnect();
});

async function handleSubmit() {
  await xenApiStore.connect(login.value, password.value);
}
</script>

<style lang="postcss" scoped>
.form-container {
  display: flex;
  align-items: center;
  flex: 1;
  justify-content: center;
  min-height: 100vh;
  max-width: 100vw;
  background-color: var(--background-color-primary);
}

form {
  display: flex;
  min-width: 30em;
  max-width: 100%;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  margin: 0 auto;
  padding: 8.5rem;
  background-color: var(--background-color-secondary);
}

h1 {
  font-size: 4.8rem;
  font-weight: 900;
  line-height: 7.2rem;
  margin-bottom: 4.2rem;
}

img {
  width: 40rem;
  margin-bottom: 5rem;
}

label {
  font-size: 120%;
  font-weight: bold;
  margin: 1.5rem 0 0.5rem 0;
}

input {
  width: 45rem;
  max-width: 100%;
  margin-bottom: 1rem;
  padding: 1rem 1.5rem;
  border: 1px solid var(--color-blue-scale-400);
  border-radius: 0.8rem;
  background-color: white;
}

button {
  margin-top: 3rem;
}
</style>
