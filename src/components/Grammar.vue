<template>
  <div class="d-flex flex-col">
    <pre>{{ contextFreeGrammar }}</pre>
    <AddEntry
      @newEntry="addNewEntry('variables', $event)"
      btnText="Adicione uma variável"
      placeholder="Nome da variável"
      style="margin-bottom: 10px"
    />

    <AddEntry
      @newEntry="addNewEntry('terminalSymbols', $event)"
      btnText="Adicione um símbolo terminal"
      placeholder="Nome do símbolo terminal"
      style="margin-bottom: 10px"
    />

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

    <div class="d-flex flex-center">
      G = (
      <div
        v-for="(target, targetIndex) in ['variables', 'terminalSymbols']"
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
        }<span v-if="targetIndex !== 1">,</span>
      </div>
      )
    </div>
  </div>
</template>

<script>
import AddEntry from "../components/AddEntry";
export default {
  components: {
    AddEntry,
  },

  data() {
    return {
      contextFreeGrammar: {
        variables: [],
        terminalSymbols: [],
        productionRules: {
          S: ["aA", "Aa", "aaa"],
          A: "b",
        },
        initialVariable: "",
      },
    };
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
  },
};
</script>

<style scoped>
.grammar-item:hover {
  text-decoration: underline !important;
  color: rgb(182, 63, 63);
  cursor: pointer !important;
}
</style>
