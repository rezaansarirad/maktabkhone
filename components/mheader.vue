<template>
  <div class="d-flex flex-column justify-center align-center bg-white">
    <v-container class="container">
      <v-row class="py-5">
        <v-col cols="2">
          <nuxt-link to="#">
            <img
              src="~/assets/images/logo.png"
              width="80"
              alt="logo-maktabkhone"
          /></nuxt-link>
        </v-col>
        <v-col cols="8">
          <v-text-field
            label="دنبال کدوم دکتری؟"
            filled
            v-model="search"
            rounded
            prepend-inner-icon="search"
          ></v-text-field>
        </v-col>
        <v-col cols="2">
          <div class="d-flex margin-top" >

          <img src="~/assets/images/account-dark.svg" width="30" alt="account-dark"
        />
        <span class="margin-right-10"> پروفایل من</span>
          </div>
          </v-col>
      </v-row>
    </v-container>
  </div>
</template>
<script>
export default {
  data() {
    return {
      search: "",
    };
  },
  watch: {
    search(val) {
      this.$emit("search", val);
    },
  },
};
</script>
