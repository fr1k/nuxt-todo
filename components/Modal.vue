<template>
  <b-form-textarea rows="3" max-rows="5" v-model="textTodo"> </b-form-textarea>
</template>

<script>
export default {
  props: {
    text: String
  },
  data() {
    return {
      textTodo: ""
    };
  },
  created() {
    this.textTodo = this.text;
  },
  watch: {
    textTodo() {
      // console.log(this.textTodo);
      this.$emit("changeTodo", this.textTodo);
    }
  }
};
</script>

<style lang="scss" scoped></style>
