<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
    >
      <div class="text-center">
        <!-- <logo /> -->
        <!-- <vuetify-logo /> -->
      </div>
      <div>
        <pageList/>
      </div>
    </v-flex>
  </v-layout>
</template>

<script>
// import Logo from '~/components/Logo.vue'
// import VuetifyLogo from '~/components/VuetifyLogo.vue'
import pageList from '~/components/pageList.vue'

export default {
  components: {
    // Logo,
    // VuetifyLogo,
    pageList,
  }
}
</script>
