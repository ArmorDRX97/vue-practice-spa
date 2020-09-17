<template>
    <div>
        <div v-if="task">
           <div class="row">
               <div class="col s6 offset-s3">
                   <h1>{{task.title}}</h1>
                   <form @submit.prevent="onSubmit">
                       <div class="chips" ref="chips"></div>
                       <div class="input-field">
                           <textarea v-model="description" id="textarea2" class="materialize-textarea min-height"></textarea>
                           <label for="textarea2">Описание</label>
                           <span class="character-counter"
                                 style="float: right; font-size: 12px;">{{description.length}}/2048</span>
                       </div>

                       <input type="text" ref="datepicker">
<div v-if="task.status !== 'completed'">
    <button class="btn" type="submit" style="margin-right: 10px;">Обновить</button>
    <button class="btn blue-grey" type="button" @click="completeTask">Завершить задачу</button>
</div>

                   </form>
               </div>
           </div>
        </div>
        <p v-else>Такой задачи не существует</p>
    </div>
</template>

<script>
    export default {
        name: "task",
        computed: {
            task() {
                return this.$store.getters.taskById(+this.$route.params.id)
            }
        },
        data: () => ({
            description: '',
            chips: null,
            date: null
        }),
        mounted() {
            this.description = this.task.description
            this.chips = window.M.Chips.init(this.$refs.chips, {
                placeholder: 'Теги задачи',
                data: this.task.tags
            })
            this.date = window.M.Datepicker.init(this.$refs.datepicker, {
                format: 'dd.mm.yyyy',
                defaultDate: new Date(this.task.date),
                setDefaultDate: true
            });

            setTimeout(() => {
                window.M.updateTextFields()
            }, 0)
        },
        methods: {
            onSubmit() {
                this.$store.dispatch('updateTask', {
                    id: this.task.id,
                    description: this.description,
                    date: this.date.date
                })
                this.$router.push('/list')
            },
            completeTask(){
                this.$store.dispatch('completeTask', this.task.id)
                this.$router.push('/list')
            }
        },
        destroyed() {
            if (this.date && this.date.destroy) {
                this.date.destroy()
            }

            if (this.chips && this.chips.destroy) {
                this.chips.destroy()
            }
        }
    }
</script>


<style lang="scss" scoped>
    .min-height{
        min-height: 150px;
    }
</style>
