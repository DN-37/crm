<template>
    <div class="container">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Регистрация</h1>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Электронная почта</label>
                        <div class="control">
                            <input type="email" name="email" class="input" v-model="username">
                        </div>
                    </div>

                    <div class="field">
                        <label>Пароль</label>
                        <div class="control">
                            <input type="password" name="password" class="input" v-model="password">
                        </div>
                    </div>

                    <div class="field">
                        <label>Повторите пароль</label>
                        <div class="control">
                            <input type="password" name="password_repeat" class="input" v-model="password_repeat">
                        </div>
                    </div>

                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-success">Отправить</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'
export default {
    name: 'SignUp',
    data() {
        return {
            username: '',
            password: '',
            password_repeat: '',
            errors: []
        }
    },
    methods: {
        async submitForm() {
            this.errors = []
            if (this.username === '') {
                this.errors.push('Имя пользователя отсутствует')
            }
            if (this.password.length < 8) {
                this.errors.push('Пароль слишком короткий')
            }
            if (this.password !== this.password_repeat) {
                this.errors.push('Пароли не совпадают')
            }
            if (!this.errors.length) {
                this.$store.commit('setIsLoading', true)

                const formData = {
                    username: this.username,
                    password: this.password
                }
                await axios
                    .post('/api/v1/users/', formData)
                    .then(response => {
                        toast({
                            message: 'Аккаунт создан, пожалуйста, войдите',
                            type: 'is-success',
                            dismissible: true,
                            pauseOnHover: true,
                            duration: 2000,
                            position: 'bottom-right',
                        })
                        this.$router.push('/log-in')
                    })
                    .catch(error => {
                        if (error.response) {
                            for (const property in error.response.data) {
                                this.errors.push(`${property}: ${error.response.data[property]}`)
                            }
                        } else if (error.message) {
                            this.errors.push('Что-то пошло не так. Пожалуйста, попробуйте еще раз!')
                        }
                    })
                this.$store.commit('setIsLoading', false)
            }
        }
    }
}
</script>