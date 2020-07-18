<template>
    <div class="">
        <h1>Список задач</h1>
        <div class="row">
            <div class="col s6">
                <select ref="select" v-model="filter">
                    <option value="" disabled selected>Выберите статус задачи</option>
                    <option value="Активна">Активные</option>
                    <option value="Завершина">Завершенные</option>
                    <option value="Просрочена">Просроченые</option>
                </select>
                <label>Materialize Select</label>
            </div>
        </div>
        <button class="btn btn-small" @click="filter = null">Отчистить фильтр</button>
        <hr>
        <table v-if="tasks.length">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Название</th>
                    <th>Срок выполнения</th>
                    <th>Описание</th>
                    <th>Статус</th>
                    <th>Редактировать</th>
                </tr>
            </thead>
            <tbody>
            <tr v-for="(task, idx) of displayTasks"
            :key="task.id"
            >
                <td>{{idx + 1}}</td>
                <td>{{task.title}}</td>
                <td>{{new Date(task.date).toLocaleDateString()}}</td>
                <td class="d-block"><div class="text">{{task.description}}</div></td>
                <td>{{task.status}}</td>
                <td>
                    <router-link
                    tag="button" class="btn btn-small" :to="'/task/' + task.id"
                    >
                    Открыть
                    </router-link>
                </td>
            </tr>
            </tbody>
        </table>
        <p v-else>No tasks</p>
    </div>
</template>

<script>
    export default {
        name: "List",
        data: () => ({
           filter: null
        }),
        computed: {
            tasks() {
                return this.$store.getters.tasks
            },
            displayTasks() {
                return this.tasks.filter(t => {
                    if(!this.filter) {
                        return true
                    }
                    return t.status === this.filter
                })
            }
        },
        mounted() {
            M.FormSelect.init(this.$refs.select, {});
        }
    }
</script>

<style scoped>
    .d-block {
        max-width: 400px;
    }
    .text {
      white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
    }
</style>