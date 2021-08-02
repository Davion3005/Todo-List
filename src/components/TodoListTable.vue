<template lang="pug">
    .card.panel-success
        .card-header List Task
        table.table.table-hover
            thead
                tr
                    th.text-center(style='width: 10%') #
                    th Task
                    th.text-center(style='width: 20%') Level
                    th(style='width: 160px') Action
            tbody(v-if="listTask.length !== 0")
                todo-item(
                    v-for="(task,index) in listTask"
                    :task="task"
                    :index="index + 1"
                    :key="index + 1"
                    v-on:onDelete="handleDelete"
                    v-on:onEdit="handleEdit")
            tbody(v-else)
                tr
                    td.text-center(colspan=4)
                        span No item found

</template>

<script>
    import TodoItem from './TodoItem'

    export default {
        name: "TodoListTable",
        components: {
            TodoItem
        },
        props: {
            listTask : {
                type: Array,
                default: () => [],
            }
        },
        data() {
            return {

            }
        },
        methods: {
            handleDelete(id) {
                this.$emit('onDelete', id);
            },
            handleEdit(task) {
                this.$emit('onEdit', task);
            }
        },
    }
</script>

<style scoped>
    .panel-success {
        margin-top: 20px;
    }
</style>
