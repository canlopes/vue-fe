<script setup lang="ts">
import { useUserStore } from "@/stores/user";
import { ref } from "vue";
import BaseForm from "./BaseForm.vue";

// Email, password, and remember me
const email = ref("");

const success = ref(false);

const baseForm = ref();

const userStore = useUserStore();
// The submit function. If there is just the email, check if the email is valid. If it is not, set the register mode. If it is, set the login mode.
const submitForm = async () => {
  const response = await userStore.sendPasswordResetEmail(email.value);
  if (response === true) {
    success.value = response;
  } else if (typeof response === "object") {
    baseForm.value.setInputErrors(response.data.errors);
  }
  return success.value;
};
</script>

<template>
  <BaseForm ref="baseForm" @submit="submitForm" :disabled="success">
    <label for="email">{{ $t("Email") }}</label>
    <input
      type="email"
      id="email"
      name="email"
      :placeholder="$t('Email')"
      v-model="email"
      :disabled="success"
      autofocus
      required
    />
    <small v-if="success" class="success">{{
      $t("If the account exists an email has been sent!")
    }}</small>
  </BaseForm>
</template>
