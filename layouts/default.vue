<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
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
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <div class="title">
        <v-toolbar-title font-size="20px" font-weight="bold" v-text="title" />
      </div>
      <v-spacer />
      <v-avatar color="indigo">HS</v-avatar>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer :absolute="!fixed" app>
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
      drawer: false,
      fixed: false,
      items: [
        {
          icon: "mdi-apps",
          title: "Welcome",
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
          title: "Data Pengiriman",
          to: "/pengiriman",
        },
        {
          icon: "mdi-cart-outline",
          title: "Data Penjualan",
          to: "/penjualan",
        },
        {
          icon: "mdi-plus",
          title: "Data Pemasukan",
          to: "/pemasukan",
        },
        {
          icon: "mdi-minus",
          title: "Data Pengeluaran",
          to: "/pengeluaran",
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
        {
          icon: "mdi-logout",
          title: "Logout",
          to: "/logout",
        },
      ],
      miniVariant: false,
      title: "MITRA USAHA",
    };
  },
};
</script>
