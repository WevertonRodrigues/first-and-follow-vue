<template>
  <div class="d-flex flex-col">
    <button v-if="!showInput" @click="handlerClickAddEntry()">
      {{ btnText }}
    </button>
    <slot v-else v-bind="{ showInput, changeShowInput }">
      <input
        :value="input"
        ref="inputAdd"
        type="text"
        :placeholder="placeholder"
        @keyup.enter.prevent="addEntry"
        @keyup.esc="$refs.inputAdd.blur()"
        @blur="changeShowInput(false)"
        @input="handler"
        maxlength="1"
      />
    </slot>
  </div>
</template>

<script>
export default {
  props: {
    btnText: {
      type: String,
      default: "Adicione",
    },
    placeholder: {
      type: String,
      default: "Nome",
    },
    inputRef: {
      type: String,
      default: "inputAdd",
    },
  },

  data() {
    return {
      input: "",
      showInput: false,
    };
  },

  watch: {
    showInput() {
      this.$emit("show");
      if (this.showInput) this.input = "";
    },
  },

  methods: {
    changeShowInput(value) {
      this.showInput = value;
    },
    handlerClickAddEntry() {
      this.changeShowInput(true);
      this.$nextTick(() => {
        this.$refs[this.inputRef].focus();
      });
    },
    addEntry(evt) {
      this.$emit("newEntry", evt.target.value);
      this.$refs.inputAdd.blur();
    },
    handler(evt) {
      this.input = evt.target.value.toLowerCase();
    },
  },
};
</script>
