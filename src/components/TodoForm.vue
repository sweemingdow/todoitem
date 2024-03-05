<template>
    <form v-on:submit.prevent="onSubmit">
        <h2 class="label-wrapper">
            <label for="new-todo-input" class="label__lg">
                What needs to be done?
            </label>
        </h2>

        <input class="input__lg" type="text" id="new-todo-input" name="new-todo" autocomplete="off" v-model.trim.lazy="lable" />
        <button type="submit" class="btn btn__primary btn__lg">
            Add
        </button>
    </form>
</template>

<script>
export default {
    methods: {
        onSubmit() {
            console.log("on submit, lable value:", this.lable)
            if (this.lable === "") return
            // 和父组件进行交互
            this.$emit("todo-added", this.lable)
            // 提交后消除输入框得值
            this.lable = ""
        }
    },
    data() {
        return {
            lable: ""
        }
    }
};
</script>