<template>
  <div class="d-flex flex-col">
    <!-- <AddEntry
      @newEntry="addNewEntry('variables', $event.toUpperCase())"
      btnText="Adicione uma variável"
      placeholder="Nome da variável"
      class="margin-bottom"
    /> -->

    <AddEntry
      @newEntry="addNewEntry('terminalSymbols', $event)"
      btnText="Adicione um símbolo terminal"
      placeholder="Nome do símbolo terminal"
      class="margin-bottom"
    />

    <AddProductionRule class="margin-bottom" @addNewRule="handlerAddRule" />

    <div class="d-flex flex-col" style="margin-bottom: 50px">
      <label for="initialVariable">Variável inicial</label>
      <select v-model="contextFreeGrammar.initialVariable">
        <option disabled value="">Escolha um item</option>
        <option
          v-for="(opt, optIndex) in initialVariableOptions"
          :key="optIndex"
          :value="opt"
        >
          {{ opt }}
        </option>
      </select>
    </div>

    <!-- Grammar -->
    <div class="d-flex flex-center margin-bottom" style="margin-bottom: 50px">
      G = (
      <div
        v-for="target in ['variables', 'terminalSymbols']"
        :key="target"
        class="d-flex"
        style="margin: 0 2px"
      >
        <span class="margin-right">{</span>
        <span
          v-for="(item, index) in contextFreeGrammar[target]"
          :key="item"
          class="grammar-item margin-right"
          @click="contextFreeGrammar[target].splice(index, 1)"
        >
          <span v-html="formatRule(item)"></span>
          <span v-if="index !== contextFreeGrammar[target].length - 1">,</span>
        </span>
        },
      </div>
      P, {{ contextFreeGrammar.initialVariable }}
      )
    </div>

    <!-- Production rules -->
    <div
      v-if="variables.length > 0"
      class="d-flex flex-center"
      style="margin-bottom: 50px"
    >
      <span class="margin-right">P = {</span>
      <div class="d-flex flex-col margin-right">
        <div
          class="d-flex"
          v-for="(
            variableProductionRule, variableProductionRuleIndex
          ) in variables"
          :key="variableProductionRuleIndex"
        >
          {{ variableProductionRule }}&rhard;
          <div
            class="d-flex"
            v-for="(rule, ruleIndex) in contextFreeGrammar.productionRules[
              variableProductionRule
            ]"
            :key="ruleIndex"
          >
            <span class="margin-x" v-html="formatRule(rule)"></span>
            <span
              v-if="
                ruleIndex !==
                contextFreeGrammar.productionRules[variableProductionRule]
                  .length -
                  1
              "
              >|</span
            >
          </div>
          <span v-if="variableProductionRuleIndex !== variables.length - 1"
            >,
          </span>
        </div>
      </div>
      }
    </div>

    <button
      @click="fillFirstsAndFollows"
      :disabled="!contextFreeGrammar.initialVariable"
      style="margin-bottom: 50px"
    >
      Preencher primeiros e seguintes
    </button>

    <div v-if="sets[0].items" class="container-result d-flex flex-center">
      <div v-for="set in sets" :key="set.name" :style="set.style">
        {{ set.name }}s
        <div
          v-for="variable in variables"
          :key="`${set.name}-${variable}`"
          class="d-flex"
        >
          <span class="margin-right">{{ set.name }}({{ variable }}) = {</span>
          <span
            v-for="(string, stringIndex) in set.items[variable]"
            :key="`${set.name}-${variable}-${string}`"
            class="margin-right"
          >
            <span v-html="formatRule(string)"></span>
            <span v-if="stringIndex !== set.items[variable].length - 1">,</span>
          </span>
          }
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AddEntry from "../components/AddEntry";
import AddProductionRule from "../components/AddProductionRule";

export default {
  components: {
    AddEntry,
    AddProductionRule,
  },

  data() {
    return {
      sets: [
        { name: "First", items: null, style: "margin-right: 15px" },
        { name: "Follow", items: null },
      ],
      contextFreeGrammar: {
        variables: ["S", "A", "B", "C", "D"],
        terminalSymbols: [],
        productionRules: {
          S: ["BA"],
          A: ["+BA", " "],
          B: ["DC"],
          C: ["*DC", " "],
          D: ["(S)", "n"],
        },
        /* 
          S → BA
          A → +BA | ''
          B → DC
          C → *DC | ''
          D → (S) | n
        */
        initialVariable: null,
      },
    };
  },

  computed: {
    initialVariableOptions() {
      return Object.keys(this.contextFreeGrammar.productionRules);
    },
    variables() {
      return Object.keys(this.contextFreeGrammar.productionRules);
    },
  },

  methods: {
    addNewEntry(target, evt) {
      if (this.contextFreeGrammar[target].findIndex((v) => v === evt) === -1) {
        if (
          [" "].includes(evt) &&
          this.contextFreeGrammar[target].includes(" ")
        )
          return;
        this.contextFreeGrammar[target].push(evt);
      }
    },

    handlerAddRule(evt) {
      const { variableName, rules } = evt;

      this.$set(this.contextFreeGrammar.productionRules, variableName, rules);
      this.contextFreeGrammar.variables.push(variableName);
    },

    formatRule(string) {
      if (![" "].includes(string)) return string;
      return "&epsilon;";
    },

    first(variable, historic = [], strings = []) {
      const rules = this.contextFreeGrammar.productionRules[variable];

      if (variable.toLowerCase() === variable) return [variable];

      for (const rule of rules) {
        if (
          rule.charAt(0) === rule.charAt(0).toUpperCase() &&
          rule.charAt(0).toLowerCase() !== rule.charAt(0).toUpperCase()
        ) {
          historic.push(variable);
          if (!historic.includes(rule.charAt(0))) {
            this.first(rule.charAt(0), historic, strings);
          }
        } else {
          /* console.log("rule", rule);
          let string = "";
          if (rule.length > 0)
            for (
              let i = 0;
              rule.charAt(i) !== rule.charAt(i).toUpperCase();
              i++
            ) {
              console.log(rule, i, rule.charAt(i));
              //string += rule.charAt(i);
            }
          else string = */
          strings.push(rule.charAt(0));
          // console.log(string);
        }
      }

      const arr = [];
      strings.map((f) => {
        if (!arr.includes(f)) arr.push(f);
      });
      return arr;
    },

    follow(variable, strings = []) {
      let index = -1;

      this.variables.map((variableM) => {
        // Rule 1
        if (
          variable === this.contextFreeGrammar.initialVariable &&
          variable === variableM
        )
          strings.push("$");
        this.contextFreeGrammar.productionRules[variableM].map(
          (productionRule) => {
            index = productionRule.indexOf(variable);

            if (index > 0) {
              let beta = [];
              beta = productionRule.slice(
                index + 1 < productionRule.length ? index + 1 : index
              );

              const firstBeta = this.first(beta[0]);
              // Rule 2
              if (beta[0] !== " " && beta[0] !== variable) {
                const firstBetaF = firstBeta.filter((fB) => fB !== " ");
                strings = [...strings, ...firstBetaF];
              }
              // Rule 3
              if (firstBeta.includes(" ") && variableM !== variable) {
                strings = [...strings, ...this.follow(variableM)];
              }
            }
          }
        );
      });

      //console.log(variable, strings);
      return strings;
    },

    fillFirstsAndFollows() {
      this.sets[0].items = {};
      this.sets[1].items = {};

      this.variables.map((variable) => {
        this.$set(this.sets[0].items, variable, this.first(variable));
        this.$set(this.sets[1].items, variable, this.follow(variable));
      });
    },
  },
};
</script>

<style scoped>
.grammar-item:hover {
  text-decoration: underline !important;
  color: rgb(182, 63, 63);
  cursor: pointer !important;
}

.margin-bottom {
  margin-bottom: 10px;
}

.container-result {
  flex-direction: row;
  border: 1px solid;
  padding: 5px;
  border-radius: 5px;
}
</style>
