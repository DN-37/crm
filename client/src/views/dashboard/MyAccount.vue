<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Мой профиль</h1>
            </div>

            <div class="column is-12">
                <button @click="logout()" class="button is-danger">Выйти</button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'MyAccount',
    methods: {
        async logout() {
            await axios
                .post('/api/v1/token/logout/')
                .then(response => {
                    console.log('Выход')
                })
                .catch(error => {
                    console.log(JSON.stringify(error))
                })

            axios.defaults.headers.common['Authorization'] = ''
            localStorage.removeItem('token')
            localStorage.removeItem('username')
            localStorage.removeItem('userid')
            localStorage.removeItem('team_name')
            localStorage.removeItem('team_id')
            this.$store.commit('removeToken')
            this.$router.push('/')
        }
    }
}
</script>