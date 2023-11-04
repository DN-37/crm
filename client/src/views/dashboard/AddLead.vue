<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Добавление лида</h1>
            </div>

            <div class="column is-12">
                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Компания</label>
                        <div class="control">
                            <input type="text" class="input" v-model="company">
                        </div>
                    </div>

                    <div class="field">
                        <label>Контакты</label>
                        <div class="control">
                            <input type="text" class="input" v-model="contact_person">
                        </div>
                    </div>

                    <div class="field">
                        <label>Электронная почта</label>
                        <div class="control">
                            <input type="email" class="input" v-model="email">
                        </div>
                    </div>

                    <div class="field">
                        <label>Телефон</label>
                        <div class="control">
                            <input type="text" class="input" v-model="phone">
                        </div>
                    </div>

                    <div class="field">
                        <label>Веб-сайт</label>
                        <div class="control">
                            <input type="text" class="input" v-model="website">
                        </div>
                    </div>

                    <div class="field">
                        <label>Доверие</label>
                        <div class="control">
                            <input type="number" class="input" v-model="confidence">
                        </div>
                    </div>

                    <div class="field">
                        <label>Расчетная стоимость</label>
                        <div class="control">
                            <input type="number" class="input" v-model="estimated_value">
                        </div>
                    </div>

                    <div class="field">
                        <label>Статус</label>
                        <div class="control">
                            <div class="select">
                                <select v-model="status">
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
                                <select v-model="priority">
                                    <option value="low">Низкий</option>
                                    <option value="medium">Средний</option>
                                    <option value="high">Высокий</option>
                                </select>
                            </div>
                        </div>
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
export default {
    name: 'AddLead',
    data() {
        return {
            company: '',
            contact_person: '',
            email: '',
            phone: '',
            estimated_value: 0,
            confidence: 0,
            website: '',
            status: 'new',
            priority: 'medium'
        }
    },
    methods: {
        async submitForm() {
            this.$store.commit('setIsLoading', true)
            const lead = {
                company: this.company,
                contact_person: this.contact_person,
                email: this.email,
                phone: this.phone,
                website: this.website,
                estimated_value: this.estimated_value,
                confidence: this.confidence,
                status: this.status,
                priority: this.priority
            }
            await axios
                .post('/api/v1/leads/', lead)
                .then(response => {
                    console.log(response)
                    this.$router.push('/dashboard/leads')
                })
                .catch(error => {
                    console.log(error)
                })
            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>   