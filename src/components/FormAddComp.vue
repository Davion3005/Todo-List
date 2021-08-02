<template lang="pug">
    .form-group.add-task
        button.btn.btn-info.btn-block(v-if="formShown" type='button' @click="onAddTaskClicked") Close
        button.btn.btn-info.btn-block(v-else type='button' @click="onAddTaskClicked") Add Task
</template>

<script>
    export default {
        name: "FormAddComp",
        props: {
            formShown: {
                type: Boolean,
                default: false,
            }
        },
        data() {
            return {

            }
        },
        methods: {
            onAddTaskClicked() {
                this.$emit('addTaskClicked')
            }
        },
    }
</script>

<style scoped>

</style>
