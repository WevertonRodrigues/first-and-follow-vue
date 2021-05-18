<template>
  <div class="d-flex flex-col">
    <button v-if="!showInput" @click="handlerClickAddEntry()">
      {{ btnText }}
    </button>
    <input
      v-else
      ref="inputAdd"
      type="text"
      :placeholder="placeholder"
      @keyup.enter.prevent="addEntry"
      @keyup.esc="$refs.inputAdd.blur()"
      @blur="showInput = false"
    />
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

  methods: {
    handlerClickAddEntry() {
      this.showInput = true;
      this.$nextTick(() => this.$refs.inputAdd.focus());
    },
    addEntry(evt) {
      this.$emit("newEntry", evt.target.value);
      this.$refs.inputAdd.blur()
    },
  },
};
</script>
