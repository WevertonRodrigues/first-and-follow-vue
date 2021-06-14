<template>
  <div>
    <AddEntry
      ref="addEntry"
      btnText="Adicione uma regra de produção"
      @show="reset()"
      inputRef="productionRuleNameInput"
    >
      <!-- Add production rule -->
      <button @click="handlerSaveProductionRules()">
        Salvar regra de produção
      </button>
      <!-- Inputs -->
      <div class="d-flex flex-center">
        <!-- Close -->
        <button @click="$refs.addEntry.changeShowInput(false)">&times;</button>
        <!-- Input variable name -->
        <input
          :value="variableName"
          ref="productionRuleNameInput"
          @keyup.esc="$refs.productionRuleNameInput.blur()"
          placeholder="Variável"
          maxlength="1"
          @input="inputVariableName"
        />
        &rhard;
        <!-- Inputs rules -->
        <div class="d-flex flex-col" style="align-items: flex-start">
          <div v-for="(rule, ruleIndex) in rules" :key="ruleIndex">
            <!-- Input -->
            <input
              :ref="`ruleInput${ruleIndex}`"
              v-model="rules[ruleIndex]"
              :placeholder="`Regra ${ruleIndex + 1}`"
              @keyup.enter.prevent="rules.push('')"
            />
            <!-- Remove btn -->
            <button @click="rules.splice(ruleIndex, 1)">&otimes;</button>
            <!-- Or span -->
            <span v-if="ruleIndex !== rules.length - 1">OU</span>
          </div>
          <!-- Add new rule btn -->
          <button
            @click="
              rules.push('');
              $nextTick(() => $refs[`ruleInput${rules.length - 1}`].focus());
            "
          >
            &plus;
          </button>
          <!-- Input add new rule -->
          <input
            ref="addNewRuleInput"
            v-if="addNewRule"
            @keyup.enter.prevent="rules.push('')"
            @keyup.esc="$refs.addNewRuleInput.blur()"
          />
        </div>
      </div>
    </AddEntry>
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
      addNewRule: false,
      variableName: "",
      rules: [],
    };
  },

  methods: {
    handlerSaveProductionRules() {
      if (this.variableName !== "" && this.rules.length > 0) {
        this.$emit("addNewRule", {
          variableName: this.variableName,
          rules: this.rules,
        });
        this.$refs.addEntry.changeShowInput(false);
      }
    },

    inputVariableName(evt) {
      this.variableName = evt.target.value.toUpperCase();
    },

    reset() {
      this.addNewRule = false;
      this.variableName = "";
      this.rules = [];
    },
  },
};
</script>
