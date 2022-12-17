<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Laporan Laba</v-card-title>
      <v-divider></v-divider>
      <v-card class="ma-5">
        <v-row>
          <template>
            <v-row>
              <v-col cols="12" sm="6">
                <v-date-picker v-model="dates" range></v-date-picker>
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="dateRangeText"
                  label="Date range"
                  prepend-icon="mdi-calendar"
                  readonly
                ></v-text-field>
                <!-- model: {{ dates }} -->
              </v-col>
              <v-btn color="success" class="mb-5 ml-7" v-on:click="getPenjualan"
                >Tampilkan</v-btn
              >
            </v-row>
          </template>
        </v-row>
        <v-row v-show="showTable">
          <v-col cols="12">
            <template>
              <v-simple-table>
                <template v-slot:default>
                  <thead>
                    <tr>
                      <th class="text-left">PENJUALAN SEBELUM PAJAK</th>
                      <th class="text-left">BIAYA POKOK</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in penjualan" :key="item.id">
                      <td>{{ item.no_do }}</td>
                      <td>{{ item.quantity }}</td>
                    </tr>
                  </tbody>
                </template>
              </v-simple-table>
            </template>
          </v-col>
        </v-row>
      </v-card>
    </v-card>
  </div>
</template>
<script>
export default {
  data: () => ({
    dates: ["2022-01-01", "2022-01-02"],
    penjualan: [],
    showTable: false,
  }),
  computed: {
    dateRangeText() {
      return this.dates;
    },
  },
  methods: {
    async getPenjualan() {
      const resultPenjualan = await this.$axios.post("/laporan-penjualan", {
        from_tanggal: this.dates[0] + "T13:00:00",
        to_tanggal: this.dates[1] + "T13:00:00",
      });
      console.log(resultPenjualan.data);
      this.penjualan = resultPenjualan.data;
    },
  },
};
</script>
<style lang=""></style>
