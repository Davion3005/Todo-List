<template lang="pug">
    b-col(cols="12")
        .form-group
            b-dropdown(text='SORT BY')
                b-dropdown-item(@click="handleSort('name', 'asc')") Name ASC
                b-dropdown-item(@click="handleSort('name', 'desc')") Name DESC
                b-dropdown-divider
                b-dropdown-item(@click="handleSort('level', 'asc')") Level ASC
                b-dropdown-item(@click="handleSort('level', 'desc')") Level DESC
            span.badge.badge-success.badge-medium.sort-content {{ sortBy }} - {{ sortDir }}
</template>

<script>
    export default {
        name: "SortComp",
        props: {
            sortBy: {
                type: String,
                default: 'name',
            },
            sortDir: {
                type: String,
                default: 'asc',
            },
        },
        data() {
            return {

            }
        },
        methods: {
            handleSort(sortBy, sortDir) {
                this.$emit('handleSort', sortBy, sortDir)
            }
        },
    }
</script>

<style scoped>
    .sort-content {
        text-transform: uppercase;
    }
</style>
