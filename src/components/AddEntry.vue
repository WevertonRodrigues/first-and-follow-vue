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
    handlerInput: {
      type: Function,
      default: (evt) => {
        console.log(evt);
        evt.target.value.toUpperCase();
      },
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
    },
  },

  methods: {
    changeShowInput(value) {
      this.showInput = value;
    },
    handlerClickAddEntry() {
      this.changeShowInput(true);
      this.$nextTick(() => {
        console.log(this.$refs[this.inputRef]);
        this.$refs[this.inputRef].focus();
      });
    },
    addEntry(evt) {
      this.$emit("newEntry", evt.target.value);
      this.$refs.inputAdd.blur();
    },
    handler(evt) {
      this.input = this.handlerInput(evt);
    },
  },
};
</script>
