<template>
    <div>
        <h1>Список задач</h1>
        <div class="row">
            <div class="col s6">
                <select ref="select" v-model="filter">
                    <option value="" disabled selected>Фильтр по статусам</option>
                    <option value="active">Активная</option>
                    <option value="outdated">Просроченная</option>
                    <option value="completed">Завершенная</option>
                </select>
                <label>Materialize Select</label>
            </div>
        </div>
        <button class="btn btn-small red" @click="filter = null" v-if="filter">Очистить фильтр</button>

        <hr>
        <table v-if="tasks.length">
            <thead>
            <tr>
                <th>#</th>
                <th>Название</th>
                <th>Дата</th>
                <th>Описание</th>
                <th>Статус</th>
                <th></th>
            </tr>
            </thead>
            <tbody>
            <tr
                    v-for="(task, index) in displayTasks"
                    :key="task.id"
            >
                <td>{{index + 1}}</td>
                <td>{{task.title}}</td>
                <td>{{ new Date(task.date).toLocaleDateString() }}</td>
                <td class="desc-td">
                    <div class="desc">{{task.description}}</div>
                </td>
                <td>{{task.status}}</td>
                <td>
                    <router-link tag="button" class="btn btn-small" :to="'/task/' + task.id">
                        Открыть
                    </router-link>
                </td>
            </tr>
            </tbody>
        </table>
        <p v-else>Задач нет</p>
    </div>
</template>

<script>
    export default {
        name: 'list',
        data: () => ({
            filter: null
        }),
        computed: {
            tasks() {
                return this.$store.getters.tasks
            },
            displayTasks() {
                return this.tasks.filter(t => {
                    if (!this.filter) {
                        return true
                    }

                    return t.status === this.filter
                })
            }
        },
        mounted() {
            window.M.FormSelect.init(this.$refs.select)
        }
    }
</script>


<style lang="scss" scoped>
    .desc {
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
    }

    .desc-td {
        max-width: 400px;
    }
</style>
