<template>
  <div>
    {{ variableName }}|{{ rules }}
    <AddEntry
      ref="addEntry"
      btnText="Adicione uma regra de produção"
      @show="reset()"
      @hide="$nextTick(() => $refs.productionRuleNameInput.focus())"
    >
      <template #default="{ showInput, changeShowInput }">
        {{ showInput }}
        <!-- Add production rule -->
        <button @click="handlerSaveProductionRules()">
          Salvar regra de produção
        </button>
        <!-- Inputs -->
        <div class="d-flex flex-center">
          <!-- Close -->
          <button @click="changeShowInput(false)">&times;</button>
          <!-- Input variable name -->
          <input
            :value="variableName"
            ref="productionRuleNameInput"
            @keyup.enter.prevent="handlerVariableName"
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
              @keyup.enter.prevent="handlerVariableName"
              @keyup.esc="$refs.addNewRuleInput.blur()"
            />
          </div>
        </div>
      </template>
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
      this.$emit("addNewRule", { [this.variableName]: this.rules });
      this.$refs.addEntry.changeShowInput(false);
    },

    inputVariableName(evt) {
      console.log(evt.target.value);
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
