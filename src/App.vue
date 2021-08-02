<template lang="pug">
    #app
        b-container
            title-comp
            b-row
                b-col(cols="12" lg="6")
                    .b-row
                        sort-comp(
                            :sortBy="sortBy"
                            :sortDir="sortDir"
                            v-on:handleSort="handleSort"
                        )
                        search-comp(
                            :strSearch="strSearch"
                            v-on:clearSearch="handleClearSearch"
                            v-on:handleSearch="handleSearch")
                        form-comp(
                            :formShown="formShown"
                            :selectedTask="selectedTask"
                            v-on:cancelClicked="onCancelClicked"
                            v-on:addTaskClicked="onAddTaskClicked"
                            v-on:onSubmit="handleSubmit"
                            v-on:onTaskEdit="handleTaskEdit")
                        todo-list-table(
                            :listTask="sortedItems"
                            v-on:onDelete="handleDelete"
                            v-on:onEdit="handleEdit")

</template>

<script>
    import TodoListTable from './components/TodoListTable'
    import FormComp from './components/FormComp'
    import TitleComp from './components/TitleComp'
    import SortComp from './components/SortComp'
    import SearchComp from './components/SearchComp'

    import listTask from './mocks/task'
    import mapLevel from './mocks/level'

    export default {
        name: 'App',
        components: {
            SearchComp,
            SortComp,
            TodoListTable,
            FormComp,
            TitleComp,
        },
        data() {
            return {
                listTask : listTask,
                mapLevel: mapLevel,
                formShown: false,
                strSearch: '',
                sortBy: 'level',
                sortDir: 'asc',
                selectedTask: null,
            }
        },
        computed: {
            searchedItems() {
                const {strSearch} = this;
                let items = [];
                this.listTask.forEach(function (item) {
                    if (item.name.toLowerCase().includes(strSearch.toLowerCase())) {
                        items.push(item)
                    }
                });
                return items;
            },
            sortedItems() {
                let items = [...this.searchedItems];
                items.sort(this.compareFunction);

                return items;
            }
        },
        created() {
            let tasks = localStorage.getItem('items')
            if (tasks !== null) {
                this.listTask = JSON.parse(tasks)
            }
            else {
                this.listTask = []
            }
        },
        watch: {
            listTask: function (newData) {
                localStorage.setItem('items', JSON.stringify(newData))
            }
        },
        methods: {
            onAddTaskClicked() {
                this.formShown = !this.formShown;
            },
            onCancelClicked() {
                this.formShown = !this.formShown;
            },
            handleSearch(data) {
                this.strSearch = data;
            },
            handleClearSearch() {
                this.strSearch = '';
            },
            handleSort(sortBy, sortDir) {
                this.sortBy = sortBy
                this.sortDir = sortDir
            },
            compareFunction(a, b) {
                const sortBy = this.sortBy
                let sortNumber = this.sortDir === 'asc' ? -1 : 1;
                if (a[sortBy] < b[sortBy]) {
                    return sortNumber;
                }
                if (a[sortBy] > b[sortBy]) {
                    return -sortNumber;
                }
                return 0;
            },
            handleDelete(id) {
                this.listTask = this.listTask.filter(task => task.id !== id);
            },
            handleSubmit(task) {
                this.listTask.push(task);
            },
            handleEdit(task) {
                this.formShown = true
                this.selectedTask = task
            },
            handleTaskEdit(editedTask) {
                let index = this.listTask.findIndex(task => task.id === editedTask.id)
                if (index !== -1) {
                    this.listTask.splice(index, 1, editedTask)
                }
            }
        },
    }
</script>

<style>
    body {
        padding: 100px 0;
    }
    .table>tbody>tr>td, .table>tbody>tr>th, .table>tfoot>tr>td, .table>tfoot>tr>th, .table>thead>tr>td, .table>thead>tr>th {
        vertical-align: middle;
    }

    .container > .row {
        margin-top: 20px;
        margin-bottom: 30px;
    }

    span.badge-medium {
        padding: 11px 10px;
        margin: 0px 8px;
        font-size: 16px;
        display: inline-block;
        vertical-align: top;
    }

    @media (max-width: 992px) {
        .add-task {
            margin-top: 50px;
        }
    }

</style>
