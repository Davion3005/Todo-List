<template lang="pug">
    tr
        td.text-center {{ index }}
        td {{ task.name }}
        td.text-center
            span.badge(:class="levelClass") {{ levelName }}
        td
            button.btn.btn-warning(type='button' @click="onEditClicked") Edit
            button.btn.btn-danger(type='button' @click="onDeleteClicked") Delete
</template>

<script>
    import mapLevel from '../mocks/level'

    export default {
        name: "TodoItem",
        props: {
            task: {
                type: Object,
                default: () => {}
            },
            index: {
                type: Number,
                default: 1,
            }
        },
        data() {
            return {
                mapLevel: mapLevel
            }
        },
        computed: {
            levelName() {
                return this.mapLevel[this.task.level].name
            },
            levelClass() {
                return this.mapLevel[this.task.level].class
            },
        },
        methods: {
            onDeleteClicked() {
                if (confirm('Would you like to delete this item?')) {
                    this.$emit('onDelete', this.task.id)
                }

            },
            onEditClicked() {
                this.$emit('onEdit', this.task)
            }
        },
    }
</script>

<style scoped>

</style>
