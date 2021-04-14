<template>
  <v-app id="inspire">
    <v-navigation-drawer app class="pt-4" color="grey lighten-3" mini-variant>
      <v-avatar size="40" class="d-block text-center mx-auto mb-9">
        <img src="./assets/logo.png" />
      </v-avatar>
      <v-avatar
        v-for="n in 5"
        :key="n"
        :color="`grey ${n === 1 ? 'darken' : 'lighten'}-1`"
        :size="n === 1 ? 30 : 20"
        class="d-block text-center mx-auto mb-9"
      ></v-avatar>
    </v-navigation-drawer>

    <v-main>
      <v-row style="margin: 10px 30px 10px 0">
        <v-col cols="12" sm="9" md="9" class="div-main">
          <!-- Se -->
          <div class="div-step" style="border-left: 5px solid orange;">
            <div class="div-nav">
              <span class="titlet offset" style="color:orange">Se</span>
              <v-btn disabled color="orange" outlined
                >Evento
                <v-icon right>
                  mdi-calendar
                </v-icon></v-btn
              >
              <v-btn disabled color="orange" outlined
                >Timer
                <v-icon right>
                  mdi-timer
                </v-icon></v-btn
              >
            </div>

            <div class="div-items">
              <IfConditionLine
                :hasConjunction="i != 0"
                :passedKey="i"
                v-for="(conditionLine, i) in ifConditionLines"
                :key="i"
                @emitIfSelection="emitIfSelection"
              />
              <div class="add-line">
                <v-chip
                  @click="
                    ifConditionLines++;
                    addConditionsOutput('if');
                  "
                  outlined
                  >+</v-chip
                >
              </div>
            </div>
          </div>

          <!-- Allora -->
          <div class="div-step" style="border-left: 5px solid #2196F3;">
            <div class="div-nav">
              <span
                class="titlet"
                style="color:#2196F3;  margin-left: -100px; margin-right: 100px;"
                >Allora</span
              >
              <v-btn
                color="blue"
                :outlined="thenSellOn"
                :dark="thenBuyOn"
                @click="
                  thenBuyOn = true;
                  thenSellOn = false;
                "
                >Compra <v-icon right>mdi-flash</v-icon></v-btn
              >
              <v-btn
                color="blue"
                :outlined="thenBuyOn"
                :dark="thenSellOn"
                @click="
                  thenSellOn = true;
                  thenBuyOn = false;
                "
                >Vendi <v-icon right>mdi-cash</v-icon></v-btn
              >
            </div>

            <div class="div-items">
              <ThenConditionLine
                v-for="(conditionLine, i) in thenConditionLines"
                :key="i"
                @emitThenSelection="emitThenSelection"
                :hasConjunction="i != 0"
                :passedKey="i"
              />
              <div class="add-line">
                <v-chip
                  @click="
                    thenConditionLines++;
                    addConditionsOutput('then');
                  "
                  outlined
                  >+</v-chip
                >
              </div>
            </div>
          </div>

          <!-- Esegui -->
          <div class="div-step" style="border-left: 5px solid purple;">
            <div class="div-nav">
              <span
                class="titlet"
                style="color:purple; margin-left: -100px; margin-right: 100px;"
                >Esegui</span
              >
            </div>
            <div class="div-items">
              <ExecuteConditionLine
                @emitExecuteSelection="emitExecuteSelection"
                @emitLaunch="emitLaunch"
              />
            </div>
          </div>

          <v-btn outlined depressed>
            Copia Pinescript
          </v-btn>
          &nbsp;
          <v-btn depressed>
            <v-icon>mdi-share</v-icon>
          </v-btn>
          <br /><br />
          <div
            style="color: #fff; background-color: #000; padding: 20px; font-family: monospace"
          >
            <span v-html="output"></span>
          </div>
        </v-col>

        <!-- Condition box -->
        <v-col cols="12" sm="3" md="3" class="div-lateral">
          <v-card class="mx-auto" max-width="344">
            <v-card-text>
              <div class="block" id="block-if">
                <!-- If -->
                <span class="orange-span">Se</span>
                <div v-for="(el, i) in conditionsOutput.if" :key="i">
                  <span v-if="el.conj != ''"
                    ><v-chip small color="primary">{{ el.conj }}</v-chip>
                    &nbsp;</span
                  >
                  <span v-if="el.act != ''"
                    ><span class="titlet">{{ el.act }}</span> è
                  </span>
                  <span v-if="el.dim != ''"
                    ><span class="titlet">{{ el.dim }}</span></span
                  >
                  <span v-if="el.val != ''">
                    <span class="titlet"> {{ el.val }}</span></span
                  >
                </div>
              </div>
              <!-- Then -->
              <div class="block" id="block-then">
                <span class="blue-span">Allora</span>
                <span
                  v-if="thenBuyOn"
                  color="blue"
                  outlined
                  @click="thenBuyOn = true"
                  >Compra <v-icon right>mdi-flash</v-icon></span
                >
                <span
                  v-if="thenSellOn"
                  color="blue"
                  outlined
                  @click="thenSellOn = true"
                  >Vendi <v-icon right>mdi-cash</v-icon></span
                >
                <div v-for="(el, i) in conditionsOutput.then" :key="i">
                  <span v-if="i != 0"
                    ><v-chip small color="primary">E</v-chip> &nbsp;</span
                  >
                  <span v-if="el.act != ''"
                    ><span class="titlet">{{ el.qnt }} </span>
                  </span>
                  <span v-if="el.crn != ''"
                    ><span class="titlet">{{ el.crn }} </span>
                  </span>
                  <span v-if="el.act != ''"
                    ><span class="titlet">di {{ el.act }}</span></span
                  >
                </div>
              </div>
              <!-- Then -->
              <div class="block" id="block-execute">
                <span class="purple-span">Esegui</span>
                <span v-if="conditionsOutput.execute.dat != ''"
                  ><span class="titlet"
                    >Comincia {{ conditionsOutput.execute.dat }}
                  </span>
                </span>
                <span v-if="conditionsOutput.execute.tms != ''"
                  ><span class="titlet"
                    >e replica {{ conditionsOutput.execute.tms }}
                  </span>
                </span>
              </div>
            </v-card-text>
            <v-card-actions>
              <v-btn text small color="gray accent-4" @click="reveal = true">
                Copia condizioni
              </v-btn>
            </v-card-actions>
          </v-card></v-col
        >
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
import IfConditionLine from "./components/IfConditionLine.vue";
import ThenConditionLine from "./components/ThenConditionLine.vue";
import ExecuteConditionLine from "./components/ExecuteConditionLine.vue";

export default {
  components: {
    IfConditionLine,
    ThenConditionLine,
    ExecuteConditionLine,
  },
  data: () => ({
    ifEventOn: true,
    ifTimerOn: false,

    thenBuyOn: true,
    thenSellOn: false,

    ifConditionLines: 1,
    thenConditionLines: 1,

    conditionsOutput: {
      if: [
        {
          act: "",
          dim: "",
          val: "",
          conj: "",
        },
      ],
      then: [
        {
          qnt: "",
          act: "",
          crn: "",
        },
      ],
      execute: {
        dat: "",
        tms: "",
      },
    },

    output: "Il tuo PineScript apparirà qui...",
  }),
  methods: {
    emitIfSelection(e) {
      this.conditionsOutput.if[e.key][e.source] = e.value;
    },
    emitThenSelection(e) {
      console.log(e);
      this.conditionsOutput.then[e.key][e.source] = e.value;
    },
    emitExecuteSelection(e) {
      this.conditionsOutput.execute[e.source] = e.value;
    },
    emitLaunch() {
      this.output = `
      //@version=4 <br/>
      study("MACD") <br/>
      fast = 12, slow = 26 <br/>
      fastMA = ema(close, fast) <br/>
      slowMA = ema(close, slow) <br/>
      macd = fastMA - slowMA <br/>
      signal = sma(macd, 9) <br/>
      plot(macd, color=color.blue) <br/>
      plot(signal, color=color.orange)
      `;
    },

    addConditionsOutput(el) {
      const templ = {
        if: {
          act: "",
          dim: "",
          val: "",
          conj: "",
        },
        then: {
          qnt: "",
          act: "",
          crn: "",
        },
      };

      this.conditionsOutput[el].push(templ[el]);
    },
  },
};
</script>

<style>
.block {
  padding: 20px 12px;
  border-top: 1px solid #ddd;
  border-right: 1px solid #ddd;
}
#block-if {
  border-left: 5px solid orange;
}
.orange-span {
  color: orange;
  text-transform: uppercase;
  font-weight: bold;
}
#block-then {
  border-left: 5px solid #2196f3;
}
.blue-span {
  color: #2196f3;
  text-transform: uppercase;
  font-weight: bold;
}
#block-execute {
  border-left: 5px solid purple;
}
.purple-span {
  color: purple;
  text-transform: uppercase;
  font-weight: bold;
}

.div-main {
  padding: 40px 20px 40px 130px;
}
.div-lateral {
}
.div-step {
  padding-bottom: 60px;
  padding-left: 10px;
  margin-bottom: 60px;
}

.second-line,
.add-line {
  padding: 12px;
  padding-top: 28px;
}
.operator-line {
}

.v-list-item {
  cursor: pointer;
}

.div-step .v-btn {
  margin-right: 12px;
}

.div-items {
  margin: 40px 20px;
  padding: 20px;
  background-color: #f7f5f3;
  border-radius: 4px;
}

.titlet {
  text-transform: uppercase;
  font-weight: bold;
}
.offset {
  margin-left: -50px;
  margin-right: 50px;
}

.intermediate-text {
  padding-right: 20px;
}
</style>
