<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Лиды</h1>

                <router-link to="/dashboard/leads/add">Добавить лида</router-link>
            </div>

            <div class="column is-12">
                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>Компания</th>
                            <th>Контакты</th>
                            <th>Назначить</th>
                            <th>Статус</th>
                            <th></th>
                        </tr>
                    </thead>

                    <tbody>
                        <tr v-for="lead in leads" v-bind:key="lead.id">
                            <td>{{ lead.company }}</td>
                            <td>{{ lead.contact_person }}</td>
                            <td>
                                <template v-if="lead.assigned_to">{{ lead.assigned_to.username }}</template>
                            </td>
                            <td>{{ lead.status }}</td>
                            <td>
                                <router-link :to="{ name: 'Lead', params: { id: lead.id } }">Детали</router-link>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Leads',
    data() {
        return {
            leads: []
        }
    },
    mounted() {
        this.getLeads()
    },
    methods: {
        async getLeads() {
            this.$store.commit('setIsLoading', true)

            axios
                .get('/api/v1/leads/')
                .then(response => {
                    this.leads = response.data
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>