<template>
  <v-app dark style="background-color: #ededed">
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
      style="background-color: #0081c9"
    >
      <v-list style="color: white">
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <template v-slot:append>
        <div class="pa-2">
          <v-btn block v-on:click="logout"> Logout </v-btn>
        </div>
      </template>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app color="pink">
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <div class="title">
        <v-toolbar-title font-size="20px" font-weight="bold" v-text="title" />
      </div>
      <v-spacer />
      <!-- <v-avatar color="indigo">HS</v-avatar> -->
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer :absolute="fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  data() {
    return {
      clipped: true,
      drawer: true,
      fixed: false,
      items: [
        {
          icon: "mdi-apps",
          title: "Home",
          to: "/",
        },
        {
          icon: "mdi-clipboard-account-outline",
          title: "Data Karyawan",
          to: "/karyawan",
        },
        {
          icon: "mdi-account-outline",
          title: "Data Customer",
          to: "/customer",
        },
        {
          icon: "mdi-car-outline",
          title: "Data Kendaraan",
          to: "/kendaraan",
        },
        {
          icon: "mdi-truck-outline",
          title: "Data Tujuan Pengiriman",
          to: "/pengiriman",
        },
        {
          icon: "mdi-cart-outline",
          title: "Data Penjualan",
          to: "/penjualan",
        },
        // {
        //   icon: "mdi-cart-outline",
        //   title: "MU Transporter",
        //   to: "/penjualan",
        // },
        {
          icon: "mdi-plus",
          title: "Data Pemasukan",
          to: "/pemasukan_kas",
        },
        {
          icon: "mdi-minus",
          title: "Data Pengeluaran",
          to: "/pengeluaran_kas",
        },
        {
          icon: "mdi-account-outline",
          title: "Data User",
          to: "/user",
        },
        {
          icon: "mdi-chart-box-outline",
          title: "Laporan",
          to: "/laporan",
        },
        // {
        //   icon: "mdi-logout",
        //   title: "Logout",
        //   to: "/logout",
        // },
      ],
      miniVariant: false,
      title: "MITRA USAHA",
    };
  },
  methods: {
    async logout() {
      console.log("hey");
      await this.$auth.logout();
      this.$router.push("/login");
    },
  },
};
</script>
