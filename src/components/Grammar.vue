<template>
  <div class="d-flex flex-col">
    <pre>{{ contextFreeGrammar }}</pre>
    <AddEntry
      @newEntry="addNewEntry('variables', $event)"
      btnText="Adicione uma variável"
      placeholder="Nome da variável"
      class="margin-bottom"
    />

    <AddEntry
      @newEntry="addNewEntry('terminalSymbols', $event)"
      btnText="Adicione um símbolo terminal"
      placeholder="Nome do símbolo terminal"
      class="margin-bottom"
    />

    <AddProductionRule class="margin-bottom" />

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

    <div class="d-flex flex-center margin-bottom">
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

    First({{ contextFreeGrammar.initialVariable }}) =
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
          S: ["A", "B", "X"],
          A: ["abaAa", "Xaa"],
          B: ["b"],
          X: ["tAAB", "e", "F"],
          F: ["`B)", "X"],
        },
        initialVariable: "",
      },
    };
  },

  mounted() {
    this.first(this.initialVariableOptions[0]);
  },

  computed: {
    initialVariableOptions() {
      return Object.keys(this.contextFreeGrammar.productionRules);
    },
  },

  methods: {
    addNewEntry(target, evt) {
      if (this.contextFreeGrammar[target].findIndex((v) => v === evt) === -1) {
        this.contextFreeGrammar[target].push(evt);
      }
    },

    first(variable, historic = []) {
      const rules = this.contextFreeGrammar.productionRules[variable];
      for (const rule of rules) {
        if (
          rule.charAt(0) === rule.charAt(0).toUpperCase() &&
          rule.charAt(0).toLowerCase() !== rule.charAt(0).toUpperCase()
        ) {
          historic.push(variable);
          if (!historic.includes(rule.charAt(0))) {
            this.first(rule.charAt(0), historic);
          }
        } else {
          /* console.log(
            "testion",
            rule.charAt(0),
            rule.charAt(0).toLowerCase() !== rule.charAt(0).toUpperCase()
          ); */
          let string = "";
          if (rule.charAt(0) !== rule.charAt(0).toUpperCase())
            for (
              let i = 0;
              rule.charAt(i) !== rule.charAt(i).toUpperCase();
              i++
            )
              string += rule.charAt(i);
          else string = rule.charAt(0);
          console.log(string);
        }
      }
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
