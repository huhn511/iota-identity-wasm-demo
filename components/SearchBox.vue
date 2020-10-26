<template>
  <v-card flat>
    <v-card-text>
      <v-form
        ref="form"
        v-on:submit.prevent="
          $router.push({ name: 'channel-channel', params: { channel: model } })
        "
      >
        <v-text-field
          v-model="model"
          :counter="max"
          :rules="rules"
          label="DID"
        ></v-text-field>
      </v-form>
    </v-card-text>

    <v-card-actions>
      <v-spacer />

      <v-btn
        :disabled="loading"
        color="primary"
        large
        outlined
        :loading="loading"
      >
        <nuxt-link :to="{ name: 'did-did', params: { channel: model } }"
          >Read DID</nuxt-link
        >
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import { MAX_LENGTH } from "~/helpers/constants.js";

export default {
  data: () => ({
    loading: false,
    allowSpaces: false,
    max: MAX_LENGTH,
    model: "",
    fullVisible: false
  }),
  mounted() {
    this.model =
      this.$route.params.channel ||
      "did:iota:com:HbuRS48djS5PbLQciy6iE9BTdaDTBM3GxcbGdyuv3TWo";
  },
  computed: {
    rules() {
      const rules = [];
      if (this.max) {
        const rule = v =>
          (v || "").length <= this.max ||
          `A maximum of ${this.max} characters is allowed`;
        rules.push(rule);
      }
      if (!this.allowSpaces) {
        const rule = v => (v || "").indexOf(" ") < 0 || "No spaces are allowed";
        rules.push(rule);
      }
      return rules;
    }
  },
  watch: {
    max: "validateField",
    model: "validateField"
  },
  methods: {
    validateField() {
      this.$refs.form.validate();
    }
  }
};
</script>