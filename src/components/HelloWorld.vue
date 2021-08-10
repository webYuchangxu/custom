<template>
  <div>
    <PublicServiceExport :config="config"></PublicServiceExport>
    <PublicServiceExport :config="config"></PublicServiceExport>
    <PublicServiceExport :config="config"></PublicServiceExport>
    <PublicServiceExport :config="config"></PublicServiceExport>
    <PublicServiceExport :config="config"></PublicServiceExport>
    <PublicServiceExport :config="config"></PublicServiceExport>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import PublicServiceExport from "./PublicServiceExport.vue";
@Component({
  components: { PublicServiceExport },
})
export default class HelloWorld extends Vue {
  public config = {
    handler: function() {
      return new Promise((resolve) => {
        resolve("success");
      });
    },
  };
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
