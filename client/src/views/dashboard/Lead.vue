<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">{{ lead.company }}</h1>
                <router-link :to="{ name: 'EditLead', params: { id: lead.id } }"
                    class="button is-light">Редактировать</router-link>
            </div>

            <div class="column is-6">
                <div class="box">
                    <h2 class="subtitle">Детали</h2>

                    <p><strong>Статус: </strong>{{ lead.status }}</p>
                    <p><strong>Приоритет: </strong>{{ lead.priority }}</p>
                    <p><strong>Доверие: </strong>{{ lead.confidence }}</p>
                    <p><strong>Расчетная стоимость: </strong>{{ lead.estimated_value }}</p>
                </div>
            </div>

            <div class="column is-6">
                <div class="box">
                    <h2 class="subtitle">Контактная информация</h2>

                    <p><strong>Контакты: </strong>{{ lead.contact_person }}</p>
                    <p><strong>Электронная почта: </strong>{{ lead.email }}</p>
                    <p><strong>Номер телефона: </strong>{{ lead.phone }}</p>
                    <p><strong>Веб-сайт: </strong>{{ lead.website }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Lead',
    data() {
        return {
            lead: {}
        }
    },
    mounted() {
        this.getLead()
    },
    methods: {
        async getLead() {
            this.$store.commit('setIsLoading', true)
            const leadID = this.$route.params.id
            axios
                .get(`/api/v1/leads/${leadID}/`)
                .then(response => {
                    this.lead = response.data
                })
                .catch(error => {
                    console.log(error)
                })
            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>