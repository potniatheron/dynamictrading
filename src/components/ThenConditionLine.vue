<template>
  <div class="div-condition-line">
    <div class="first-line">
      <div v-if="hasConjunction" class="conjunction">
        <v-chip color="primary" v-bind="attrs" v-on="on">
          E
        </v-chip>
      </div>

      <!-- -->
      <v-row>
        <v-col cols="12" sm="7" md="7">
          <v-text-field
            @input="emitSelection(currencyQuantity, 'qnt')"
            v-model="currencyQuantity"
            dense
            label="Quantità"
            solo
          >
            <template v-slot:append>
              <v-menu :offset-y="true">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn color="grey" dark v-bind="attrs" v-on="on">
                    {{ currencyItemsSelected }}
                  </v-btn>
                </template>
                <v-list>
                  <v-list-item v-for="(item, i) in currencyItems" :key="i">
                    <v-list-item-title
                      @click="
                        currencyItemsSelected = item.title;
                        emitSelection(item.title, 'crn');
                      "
                      >{{ item.title }}</v-list-item-title
                    >
                  </v-list-item>
                </v-list>
              </v-menu>
            </template>
          </v-text-field>
        </v-col>

        <!-- Valore -->
        <v-col>
          <span class="intermediate-text">di</span>
          <v-menu :offset-y="true">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="grey" dark v-bind="attrs" v-on="on">
                {{ valueItemsSelected }}
              </v-btn>
            </template>
            <v-list>
              <v-list-item v-for="(item, i) in valueItems" :key="i">
                <v-list-item-title
                  @click="
                    valueItemsSelected = item.title;
                    emitSelection(item.title, 'act');
                  "
                  >{{ item.title }}</v-list-item-title
                >
              </v-list-item>
            </v-list>
          </v-menu></v-col
        >
      </v-row>
    </div>
  </div>
</template>
<script>
export default {
  props: ["hasConjunction", "passedKey"],

  data: () => ({
    currencyQuantity: 0,
    currencyItemsSelected: "Moneta",
    currencyItems: [{ title: "$" }, { title: "€" }, { title: "£" }],

    valueItemsSelected: "Azione",
    valueItems: [
      { title: "Azione 1" },
      { title: "Azione 2" },
      { title: "Azione 3" },
      { title: "Azione 4" },
    ],

    operatorSelected: "Operatore",
    operators: [{ title: "E" }, { title: "OPPURE" }],
  }),

  methods: {
    emitSelection(value, source) {
      const obj = {
        key: this.passedKey,
        source,
        value,
      };

      this.$emit("emitThenSelection", obj);
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
