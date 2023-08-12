<template>
  <locale-switcher v-show="false"/>
  <div class="container"></div>
  <router-view></router-view>
</template>

<script>
import { useI18n } from 'vue-i18n';
import LocaleSwitcher from './components/LocaleSwitcher.vue';
export default {
  name: "App",
  components: {
    LocaleSwitcher
  },
  setup() {
    const { t } = useI18n({
        inheritLocale: true,
        useScope: "global"
    });
    return { t };
  },
}
</script>

<style>
* {
  font-family: Ubuntu, sans-serif;
}
body {
  margin:0;
  padding:0;
  font-size:16px;
  color:#fefefe;
}
</style>
