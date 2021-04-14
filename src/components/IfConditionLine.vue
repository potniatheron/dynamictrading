<template>
  <div class="div-condition-line">
    <div class="first-line">
      <div v-if="hasConjunction" class="conjunction">
        <v-menu :offset-y=true>
          <template v-slot:activator="{ on, attrs }">
            <div class="operator-line">
              <v-chip color="primary" v-bind="attrs" v-on="on">
                {{ operatorSelected }}
              </v-chip>
            </div>
          </template>
          <v-list>
            <v-list-item v-for="(item, i) in operators" :key="i">
              <v-list-item-title
                @click="
                  operatorSelected = item.title;
                  emitSelection(operatorSelected, 'conj');
                "
                >{{ item.title }}</v-list-item-title
              >
            </v-list-item>
          </v-list>
        </v-menu>
      </div>
      <!-- Azione -->
      <v-menu :offset-y=true>
        <template v-slot:activator="{ on, attrs }">
          <v-btn color="grey" dark v-bind="attrs" v-on="on">
            {{ entityItemsSelect }}
          </v-btn>
        </template>
        <v-list>
          <v-list-item v-for="(item, i) in entityItems" :key="i">
            <v-list-item-title
              @click="
                entityItemsSelect = item.title;
                emitSelection(item.title, 'act');
              "
              >{{ item.title }}</v-list-item-title
            >
          </v-list-item>
        </v-list>
      </v-menu>
      <span class="intermediate-text">è</span>

      <!-- Posizione -->
      <v-menu :offset-y=true>
        <template v-slot:activator="{ on, attrs }">
          <v-btn color="grey" dark v-bind="attrs" v-on="on">
            {{ sizeItemsSelected }}
          </v-btn>
        </template>
        <v-list>
          <v-list-item v-for="(item, i) in sizeItems" :key="i">
            <v-list-item-title
              @click="
                sizeItemsSelected = item.title;
                emitSelection(item.title, 'dim');
              "
              >{{ item.title }}</v-list-item-title
            >
          </v-list-item>
        </v-list>
      </v-menu>
      <span class="intermediate-text">di</span>

      <!-- Valore -->
      <v-menu :offset-y=true>
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
                emitSelection(item.title, 'val');
              "
              >{{ item.title }}</v-list-item-title
            >
          </v-list-item>
        </v-list>
      </v-menu>
    </div>
  </div>
</template>
<script>
export default {
  props: ["hasConjunction", "passedKey"],

  data: () => ({
    entityItemsSelect: "Azione",
    entityItems: [
      { title: "Azione 1" },
      { title: "Azione 2" },
      { title: "Azione 3" },
      { title: "Azione 4" },
    ],
    sizeItemsSelected: "Dimensioni",
    sizeItems: [
      { title: "maggiore di" },
      { title: "minore di" },
      { title: "uguale a" },
    ],
    valueItemsSelected: "Valore",
    valueItems: [
      { title: 100 },
      { title: 200 },
      { title: 300 },
      { title: 400 },
    ],

    //
    operatorSelected: "Operatore",
    operators: [{ title: "E" }, { title: "OPPURE" }],

    offset: true,
  }),

  methods: {
    emitSelection(value, source) {
      const obj = {
        key: this.passedKey,
        source,
        value,
      };

      this.$emit("emitIfSelection", obj);
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
