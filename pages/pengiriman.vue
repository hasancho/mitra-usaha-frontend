<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Data Pengiriman</v-card-title>
      <v-divider></v-divider>
      <v-card class="ma-5">
        <v-form ref="form" class="pa-5" v-model="valid" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="tujuan"
                label="Tujuan"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="tarif"
                label="Tarif"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="name"
                label="Biaya Pokok"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="name"
                label="Material"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-btn color="success" class="mr-4"> Submit </v-btn>
          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    tujuan: "",
    tarif: "",
    biaya_pokok: "",
    material: "",
    name: "",
    value: null,
    items: ["Item 1", "Item 2", "Item 3", "Item 4"],
  }),

  methods: {
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
