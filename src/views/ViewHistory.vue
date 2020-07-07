<template>
  <main class="app-main view-history">
    <div class="columns middle" style="margin-bottom:.4rem">
      <h1 class="expanded">ประวัติสถานที่</h1>
      <div @click="clearHistory()"><i class="fas fa-trash-alt fa-2x"></i></div>
    </div>
    <app-empty-state v-if="!getHistory.length" name="ประวัติสถานที่" />
    <app-list-item
      v-for="(shop, index) in getHistory"
      :key="index"
      v-bind="shop"
      @click.native="gotoShop(shop)"
      @remove="removeHistory"
      removable
    />
  </main>
</template>

<script>
import AppEmptyState from "@/components/AppEmptyState";
import AppListItem from "@/components/AppListItem";
import storeGetter from "@/store/getter.js";

export default {
  name: "ViewHistory",
  components: {
    AppListItem,
    AppEmptyState
  },
  computed: {
    ...storeGetter
  },
  methods: {
    gotoShop(shopData) {
      this.$store.dispatch("setDetail", shopData);
      this.$router.push("/shop_detail");
    },
    clearHistory() {
      if (
        confirm(
          "ต้องการล้างประวัติสถานที่หรือไม่?\nข้อมูลสถานที่ทั้งหมดจะถูกลบ และไม่สามารถกู้คืนได้!"
        )
      )
        this.$store.dispatch("clearHistory");
    }
  }
};
</script>

<style scoped>
.view-history {
  margin-top: 1rem;
}
</style>
