<template lang="pug">
    b-col(cols="12")
        .input-group
            input.form-control(
                :value="strSearch"
                @input="handleSearch"
                type='text'
                placeholder='Search for...')
            span.input-group-append
                button.btn.btn-info(
                    @click="onClearSearch"
                    type='button') Clear!
</template>

<script>
    export default {
        name: "SearchComp",
        props: {
            strSearch: {
                type: String,
                default: '',
            },
        },
        data() {
            return {

            }
        },
        methods: {
            handleSearch(event) {
                this.$emit('handleSearch', event.target.value)
            },
            onClearSearch() {
                this.$emit('clearSearch')
            }
        },
    }
</script>

<style scoped>

</style>
