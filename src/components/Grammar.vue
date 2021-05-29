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
          {{ item
          }}<span v-if="index !== contextFreeGrammar[target].length - 1"
            >,</span
          >
        </span>
        },
      </div>
      P, {{ contextFreeGrammar.initialVariable }}
      )
    </div>

    <!-- Production rules -->
    <div
      v-if="Object.keys(contextFreeGrammar.productionRules).length > 0"
      class="d-flex flex-center"
      style="margin-bottom: 50px"
    >
      <span class="margin-right">P = {</span>
      <div class="d-flex flex-col margin-right">
        <div
          class="d-flex"
          v-for="(
            variableProductionRule, variableProductionRuleIndex
          ) in Object.keys(contextFreeGrammar.productionRules)"
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
          <span
            v-if="
              variableProductionRuleIndex !==
              Object.keys(contextFreeGrammar.productionRules).length - 1
            "
            >,
          </span>
        </div>
      </div>
      }
    </div>

    <!-- First -->
    <div
      v-if="contextFreeGrammar.initialVariable.length > 0"
      class="d-flex flex-center"
    >
      <span class="margin-right"
        >First({{ contextFreeGrammar.initialVariable }}) = {</span
      >
      <span
        v-for="(string, stringIndex) in firstFromInitialVariable"
        :key="string"
        class="margin-right"
      >
        <span v-html="formatRule(string)"></span>
        <span v-if="stringIndex !== firstFromInitialVariable.length - 1"
          >,</span
        >
      </span>
      }
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
      contextFreeGrammar: {
        variables: [],
        terminalSymbols: [],
        productionRules: {
          /* S: ["A", "B", "X", "cF"],
          A: ["abaAa", "Xaa"],
          B: ["b", ""],
          X: ["2AAB", "e", "F"],
          F: ["`B)", "X", "B"], */
        },
        initialVariable: "",
      },
    };
  },

  computed: {
    initialVariableOptions() {
      return Object.keys(this.contextFreeGrammar.productionRules);
    },
    firstFromInitialVariable() {
      let arr = [];
      if (this.contextFreeGrammar.initialVariable !== "") {
        const temp = this.first(this.contextFreeGrammar.initialVariable);

        temp.map((f) => {
          if (!arr.includes(f)) arr.push(f);
        });
      }
      return arr;
    },
  },

  methods: {
    addNewEntry(target, evt) {
      if (this.contextFreeGrammar[target].findIndex((v) => v === evt) === -1) {
        this.contextFreeGrammar[target].push(evt);
      }
    },

    handlerAddRule(evt) {
      const { variableName, rules } = evt;

      this.$set(this.contextFreeGrammar.productionRules, variableName, rules);
      this.contextFreeGrammar.variables.push(variableName);
    },

    formatRule(string) {
      if (string.length > 0) return string;
      return "&epsilon;";
    },

    first(variable, historic = [], strings = []) {
      const rules = this.contextFreeGrammar.productionRules[variable];

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
      return strings;
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
</style>
