<template>
  <main class="app-main qrshare">
    <div class="qrcode">
      <vue-qr
        :logoSrc="logo"
        :bgSrc="bg"
        :text="text"
        :correctLevel="correctLevel"
        :size="size"
        :dotScale="dotScale"
      />
    </div>
    <div class="detail">
      <h2>{{ getDetail.shopName }}</h2>
      <p>{{ getDetail.businessType }}</p>
    </div>
  </main>
</template>

<script>
import VueQr from "vue-qr";
import storeGetter from "@/store/getter.js";
import logoImg from "@/assets/logo.png";
import bgImg from "@/assets/bg.png";

export default {
  name: "ShopDetail",
  data() {
    return {
      logo: logoImg,
      bg: bgImg,
      text: null,
      correctLevel: 3,
      size: 280,
      dotScale: 0.6
    };
  },
  components: {
    VueQr
  },
  computed: {
    ...storeGetter
  },
  created() {
    this.text = `https://qr.thaichana.com/?appId=${this.getDetail.appId}&shopId=${this.getDetail.shopId}`;
  }
};
</script>

<style scoped>
.qrcode {
  display: flex;
  justify-content: center;
}

.detail {
  text-align: center;
}
</style>
