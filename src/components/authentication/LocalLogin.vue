<template>
  <mv-form :model="data" :disabled="isAuthenticating" @submit="tryLocalLogin">
    <h2>Connexion locale</h2>
    <p>
      Utilisez votre identifiant et votre mot de passe pour accéder à
      l'interface d'administration.
    </p>
    <mv-form-item label="Votre identifiant" required>
      <el-input
        v-model="data.username"
        type="text"
        placeholder="adresse@email.com"
        required
      />
    </mv-form-item>
    <mv-form-item label="Votre mot de passe" required>
      <el-input
        v-model="data.password"
        type="password"
        placeholder="******"
        required
      />
    </mv-form-item>
    <p v-if="error" class="mv-localLogin-error" v-text="error"></p>
    <mv-button type="submit" icon="lock" :loading="isAuthenticating" success
      >Connexion</mv-button
    >
  </mv-form>
</template>

<script>
import { Input as ElInput } from "element-ui";
import MvButton from "../Button.vue";
import MvForm from "../forms/Form.vue";
import MvFormItem from "../forms/FormItem.vue";

export default {
  name: "MvLocalLogin",
  inject: ["authentication"],
  components: {
    ElInput,
    MvButton,
    MvForm,
    MvFormItem
  },
  data() {
    return {
      isAuthenticating: false,
      error: null,
      data: {}
    };
  },
  methods: {
    async tryLocalLogin() {
      this.isAuthenticating = true;
      try {
        await this.authentication.login({ type: "local", ...this.data });
      } catch (err) {
        this.error = err.message;
      } finally {
        this.isAuthenticating = false;
      }
    }
  }
};
</script>

<style lang="scss">
@import "../../theme/_variables.scss";

.mv-localLogin {
  &-error {
    color: $--color-danger;
    font: font-bold(14px);
    padding: 0;
    margin: 15px 0;
  }
}
</style>
