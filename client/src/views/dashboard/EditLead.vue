<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Редактировать {{ lead.company }}</h1>
            </div>

            <div class="column is-12">
                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Компания</label>
                        <div class="control">
                            <input type="text" class="input" v-model="lead.company">
                        </div>
                    </div>

                    <div class="field">
                        <label>Контакты</label>
                        <div class="control">
                            <input type="text" class="input" v-model="lead.contact_person">
                        </div>
                    </div>

                    <div class="field">
                        <label>Электронная почта</label>
                        <div class="control">
                            <input type="email" class="input" v-model="lead.email">
                        </div>
                    </div>

                    <div class="field">
                        <label>Номер телефона</label>
                        <div class="control">
                            <input type="text" class="input" v-model="lead.phone">
                        </div>
                    </div>

                    <div class="field">
                        <label>Веб-сайт</label>
                        <div class="control">
                            <input type="text" class="input" v-model="lead.website">
                        </div>
                    </div>

                    <div class="field">
                        <label>Доверие</label>
                        <div class="control">
                            <input type="number" class="input" v-model="lead.confidence">
                        </div>
                    </div>

                    <div class="field">
                        <label>Расчетная стоимость</label>
                        <div class="control">
                            <input type="number" class="input" v-model="lead.estimated_value">
                        </div>
                    </div>

                    <div class="field">
                        <label>Статус</label>
                        <div class="control">
                            <div class="select">
                                <select v-model="lead.status">
                                    <option value="new">Новый</option>
                                    <option value="inprogress">В процессе</option>
                                    <option value="lost">Отрицательный</option>
                                    <option value="won">Положительный</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <div class="field">
                        <label>Приоритет</label>
                        <div class="control">
                            <div class="select">
                                <select v-model="lead.priority">
                                    <option value="low">Низкий</option>
                                    <option value="medium">Средний</option>
                                    <option value="high">Высокий</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <div class="field">
                        <label>Назначить</label>
                        <div class="control">
                            <div class="select">
                                <select v-model="lead.assigned_to">
                                    <option value="" selected>Выбрать участника</option>
                                    <option v-for="member in team.members" v-bind:key="member.id" v-bind:value="member.id">
                                        {{ member.username }}
                                    </option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-success">Сохранить</button>
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
    name: 'EditLead',
    data() {
        return {
            lead: {},
            team: {
                members: []
            }
        }
    },
    mounted() {
        this.getLead()
        this.getTeam()
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
        },
        async getTeam() {
            this.$store.commit('setIsLoading', true)
            await axios
                .get('/api/v1/teams/get_my_team/')
                .then(response => {
                    this.team = response.data
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        },
        async submitForm() {
            this.$store.commit('setIsLoading', true)
            const leadID = this.$route.params.id
            axios
                .patch(`/api/v1/leads/${leadID}/`, this.lead)
                .then(response => {
                    toast({
                        message: 'Лид был успешно обновлен',
                        type: 'is-success',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'bottom-right',
                    })
                    this.$router.push(`/dashboard/leads/${leadID}`)
                })
                .catch(error => {
                    console.log(error)
                })
            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>