<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Data Pengeluaran</v-card-title>
      <v-divider></v-divider>
      <v-card class="ma-5">
        <v-form ref="form" class="pa-5" v-model="valid" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="tanggal"
                label="Tanggal"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="keterangan"
                label="Keterangan"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="total"
                label="Total"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field v-model="akun" label="Akun" required></v-text-field>
            </v-col>
          </v-row>
          <v-btn color="success" class="mr-4" @click="validate"> Submit </v-btn>
          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    tanggal: "",
    keterangan: "",
    total_pengeluaran: "",
    akun: "",
    value: null,
    select: null,
    items: ["Item 1", "Item 2", "Item 3", "Item 4"],
  }),

  methods: {
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
