<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Data Penjualan</v-card-title>
      <v-divider></v-divider>
      <v-card class="ma-5">
        <v-form ref="form" class="pa-5" v-model="valid" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-select
                v-model="select"
                :items="items"
                label="Customer"
                required
              ></v-select>
            </v-col>
            <v-col cols="4">
              <v-select
                v-model="select"
                :items="items"
                :rules="[(v) => !!v || 'Item is required']"
                label="Pengiriman"
                required
              ></v-select>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-select
                v-model="select"
                :items="items"
                label="Kendaraan"
                required
              ></v-select>
            </v-col>
            <v-col cols="4">
              <v-select
                v-model="select"
                :items="items"
                label="Karyawan"
                required
              ></v-select>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="name"
                label="No. Do"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="value"
                label="Tanggal"
                dense
                hide-details
                class="pa-5"
              >
                <template v-slot:append-outer>
                  <date-picker v-model="value" />
                </template>
              </v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="name"
                :rules="nameRules"
                label="Quantity"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-radio-group v-model="radioGroup" label="Pembayaran">
                <v-radio
                  v-for="s in status_pembayaran"
                  :key="s"
                  :label="s.value"
                  :value="s.value"
                ></v-radio>
              </v-radio-group>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="name"
                label="Total Biaya"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-btn
            color="success"
            class="mr-4"
          >
            Submit
          </v-btn>

          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
    </v-card>
  </div>
</template>

<script>
import DatePicker from "../components/DatePicker.vue";
export default {
  components: {
    DatePicker,
  },
  data: () => ({
    valid: true,
    name: "",
    email: "",
    value: null,
    radioGroup: "",
    select: null,
    status_pembayaran: [{ value: "cash" }, { value: "bank" }],
    items: ["Item 1", "Item 2", "Item 3", "Item 4"],
    checkbox: false,
  }),

  methods: {
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
