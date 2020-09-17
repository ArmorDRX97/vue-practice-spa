<template>
    <div class="row">
        <div class="col s6 offset-s3">
            <form @submit.prevent="onSubmit">
                <div class="input-field">
                    <input v-model="title" type="text" id="title" class="validate" required>
                    <label for="title">Заголовок</label>
                    <span class="helper-text" data-error="Заголовок обязателен"></span>
                </div>
                <div class="chips" ref="chips"></div>
                <div class="input-field">
                    <textarea v-model="description" id="textarea2" class="materialize-textarea"></textarea>
                    <label for="textarea2">Описание</label>
                    <span class="character-counter"
                          style="float: right; font-size: 12px;">{{description.length}}/2048</span>
                </div>

                <input type="text" ref="datepicker">

                <button class="btn" type="submit">Создать</button>
            </form>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Home',
        data: () => ({
            title: '',
            description: '',
            chips: null,
            date: null
        }),
        mounted() {
            this.chips = window.M.Chips.init(this.$refs.chips, {
                placeholder: 'Теги задачи'
            })
            this.date = window.M.Datepicker.init(this.$refs.datepicker, {
                format: 'dd.mm.yyyy',
                defaultDate: new Date(),
                setDefaultDate: true
            });
        },
        methods:{
            onSubmit(){
                const task = {
                    title: this.title,
                    description: this.description,
                    id: Date.now(),
                    status: 'active',
                    tags: this.chips.chipsData,
                    date: this.date.date
                }

               this.$store.dispatch('createTask', task)
                this.$router.push('/list')
            }
        },
        destroyed() {
            if(this.date && this.date.destroy){
                this.date.destroy()
            }

            if(this.chips && this.chips.destroy){
                this.chips.destroy()
            }
        }
    }
</script>
