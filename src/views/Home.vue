<template>
  <div class="home">
    <div class="qr">
      <div class="reader" style="height:50vh">
        <qrcode-stream @decode="onDecode"></qrcode-stream>
      </div>
      <app-toast v-if="was_incorrect" msg="QR Code ไม่ถูกต้อง" />
    </div>
    <hr />
    <section>
      <div class="media center" style="margin-bottom:8px">
        <h1 class="body">สถานที่ล่าสุด</h1>
        <div>
          <router-link to="/history" aria-label="ประวัติสถานที่ทั้งหมด">
            <i class="fas fa-history fa-2x"></i>
          </router-link>
        </div>
      </div>
      <app-empty-state v-if="!getTopHistory.length" name="ประวัติสถานที่" />
      <div
        v-for="(shop, index) in getTopHistory"
        :key="index"
        class="list-item"
        @click="gotoShop(shop)"
      >
        <div class="media">
          <app-shopping-icon />
          <div class="body" style="margin-left:10px">
            <h2>{{ shop.shopName }}</h2>
            <p>{{ shop.businessType }}</p>
          </div>
        </div>
      </div>
      <router-link v-if="getHistory.length > 3" to="/history">
        <div class="list-item" style="text-align:center;font-size:1.25rem">
          ดูเพิ่มเติม...
        </div>
      </router-link>
    </section>
    <hr />
    <section>
      <div class="media center" style="margin-bottom:8px">
        <h1 class="body">รายการโปรด</h1>
        <div>
          <router-link to="/favorite" aria-label="แก้ไขรายการโปรด">
            <i class="fas fa-pen fa-2x"></i>
          </router-link>
        </div>
      </div>
      <app-empty-state
        v-if="!getFavorite.length"
        name="รายการโปรด"
        description="ท่านสามารถเพิ่มสถานที่ลงในรายการโปรดได้หลังจากเช็คอินสถานที่นั้น"
      />
      <div
        v-for="(shop, index) in getFavorite"
        :key="index"
        class="list-item"
        @click="gotoShop(shop)"
      >
        <div class="media">
          <app-shopping-icon />
          <div class="body" style="margin-left:10px">
            <h2>{{ shop.shopName }}</h2>
            <p>{{ shop.businessType }}</p>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import AppShoppingIcon from "@/components/AppShoppingIcon";
import AppEmptyState from "@/components/AppEmptyState";
import AppToast from "@/components/AppToast";
import storeGetter from "@/store/getter.js";
import {QrcodeStream} from "vue-qrcode-reader";

export default {
  name: "Home",
  components: {
    AppShoppingIcon,
    AppEmptyState,
    QrcodeStream,
    AppToast
  },
  computed: {
    ...storeGetter
  },
  data() {
    return {
      was_incorrect: false
    };
  },
  methods: {
    onDecode(result) {
      console.log(result);
      if (result.indexOf("qr.thaichana.com") !== -1) {
        const url = new URL(result);
        const params = new URLSearchParams(url.search);
        let appId = params.get("appId");
        let shopId = params.get("shopId");
        fetch(`https://api-customer.thaichana.com/shop/${appId}/${shopId}/qr`)
                .then(res => res.json())
                .then(data => {
                  this.gotoShop(data);
                })
                // eslint-disable-next-line no-unused-vars
                .catch(_ => (this.was_incorrect = true));
      } else {
        this.was_incorrect = true;
      }
    },
    gotoShop(shopData) {
      this.$store.dispatch("setDetail", shopData);
      this.$router.push("/shop_detail");
    }
  },
  watch: {
    was_incorrect() {
      if (this.was_incorrect)
        setTimeout(() => (this.was_incorrect = false), 5000);
    }
  }
};
</script>

<style lang="scss" scoped>
.home {
  margin-top: 2rem;
  padding: 0 20px;
}
</style>
