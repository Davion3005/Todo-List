<template lang="pug">
    b-col(cols="12" lg="6")
        form-add-comp(
            v-on:addTaskClicked="onAddTaskClicked"
            :formShown="formShown")
        form.form-inline.justify-content-between(v-if="formShown" action='' method='POST')
            .form-group
                label.sr-only(for='') label
                input.form-control(v-model="name" type='text' placeholder='Task Name')
            .form-group
                label.sr-only(for='') label
                select.form-control(name='ds' required='required' v-model="level")
                    option(value='0') Small
                    option(value='1') Medium
                    option(value='2') High
            button.btn.btn-primary(v-if="!selectedTask" type='button' @click="onSubmitClicked") Submit
            button.btn.btn-primary(v-else type='button' @click="onEditClicked") Edit
            button.btn.btn-secondary(type='button' @click="onCancelClicked") Cancel
</template>

<script>
    import { v4 as uuid } from 'uuid'
    import FormAddComp from './FormAddComp'

    export default {
        name: "FormComp",
        components: { FormAddComp },
        props: {
            formShown: {
                default: false,
                type: Boolean,
            },
            selectedTask: {
                default: null,
                type: Object,
            }
        },
        data() {
            return {
                name: '',
                level: 0,
            }
        },
        watch: {
            selectedTask: function (newData) {
                if (newData !== null) {
                    this.name = newData.name
                    this.level = newData.level
                }
            }
        },
        methods: {
            onAddTaskClicked() {
                this.resetData()
                this.$emit('addTaskClicked')
            },
            onCancelClicked() {
                this.resetData()
                this.$emit('cancelClicked')
            },
            onSubmitClicked() {
                let newTask = {
                    id: uuid(),
                    name: this.name,
                    level: parseInt(this.level)
                };
                this.$emit('onSubmit', newTask);
                this.onCancelClicked();
            },
            onEditClicked() {
                let editedTask = {
                    id: this.selectedTask.id,
                    name: this.name,
                    level: parseInt(this.level)
                }
                this.$emit('onTaskEdit', editedTask)
                this.onCancelClicked()
            },
            resetData() {
                this.name = '';
                this.level = 0;
            },
        },
    }
</script>

<style scoped>

</style>
