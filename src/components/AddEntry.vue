<template>
  <div class="d-flex flex-col">
    <button v-if="!showInput" @click="handlerClickAddEntry()">
      {{ btnText }}
    </button>
    <slot v-else v-bind="{ showInput, changeShowInput }">
      <input
        ref="inputAdd"
        type="text"
        :placeholder="placeholder"
        @keyup.enter.prevent="addEntry"
        @keyup.esc="$refs.inputAdd.blur()"
        @blur="changeShowInput(false)"
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
  },

  data() {
    return {
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
      console.log(this.$slots.default);
      if (this.$slots.default !== undefined)
        this.$nextTick(() => this.$refs.inputAdd.focus());
      else this.$emit("hide");
    },
    addEntry(evt) {
      this.$emit("newEntry", evt.target.value);
      this.$refs.inputAdd.blur();
    },
  },
};
</script>
