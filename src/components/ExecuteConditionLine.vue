<template>
  <div class="div-condition-line">
    <div class="first-line">
      <!-- -->
      <v-row>
        <v-col cols="12" sm="2" md="2">
          <span>Comincia</span>
        </v-col>
        <v-col cols="12" sm="3" md="3">
          <v-text-field
            @input="emitSelection(calendarData, 'dat')"
            v-model="calendarData"
            dense
            label="Data"
            solo
          >
          </v-text-field>
        </v-col>

        <v-col cols="12" sm="2" md="2">
          <span>ed esegui</span>
        </v-col>
        <v-col cols="12" sm="3" md="3">
          <v-text-field
            @input="emitSelection(numberTimes, 'tms')"
            v-model="numberTimes"
            dense
            label="Numero"
            solo
          >
          </v-text-field>
        </v-col>
        <v-col cols="12" sm="2" md="2">
          <span>volte</span>
        </v-col>
      </v-row>
    </div>
    <v-btn rounded color="purple" dark @click="$emit('emitLaunch')"
      >Lancia!</v-btn
    >
  </div>
</template>
<script>
export default {
  data: () => ({
    calendarData: "",
    numberTimes: "",
  }),

  methods: {
    emitSelection(value, source) {
      const obj = {
        source,
        value,
      };

      this.$emit("emitExecuteSelection", obj);
    },
  },
};
</script>
<style>
.div-condition-line {
  padding: 12px;
}
.conjunction {
  padding-bottom: 20px;
}
</style>
