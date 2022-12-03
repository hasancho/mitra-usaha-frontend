<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Laporan Pemasukan Kas</v-card-title>
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
                model: {{ dates }}
              </v-col>
              <v-btn color="success" class="mb-5 ml-7" v-on:click="getPemasukan"
                >Tampilkan</v-btn
              >
            </v-row>
          </template>
        </v-row>
        <v-row>
          <v-col cols="12">
            <template>
              <v-simple-table>
                <template v-slot:default>
                  <thead>
                    <tr>
                      <th class="text-left">Tanggal</th>
                      <th class="text-left">Katerangan</th>
                      <th class="text-left">Total</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in pemasukan" :key="item.id">
                      <td>{{ item.tanggal }}</td>
                      <td>{{ item.keterangan }}</td>
                      <td>{{ item.total_pemasukan_kas }}</td>
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
    pemasukan: [],
  }),
  computed: {
    dateRangeText() {
      return this.dates;
    },
  },
  methods: {
    async getPemasukan() {
      const result = await this.$axios.post("/laporan-pemasukan", {
        from_tanggal: this.dates[0] + "T13:00:00",
        to_tanggal: this.dates[1] + "T13:00:00",
      });
      this.pemasukan = result.data;
    },
  },
};
</script>
<style lang=""></style>
