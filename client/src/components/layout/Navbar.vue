<template>
    <nav class="navbar is-dark">
        <div class="navbar-brand">
            <router-link to="/" class="navbar-item">
                <strong>CRM</strong>
            </router-link>
        </div>

        <div class="navbar-menu">
            <div class="navbar-end">
                <router-link to="/dashboard/leads" class="navbar-item">Руководители</router-link>
                <div class="navbar-item">
                    <div class="buttons">
                        <router-link to="/sign-up" class="button is-success"><strong>Регистрация</strong></router-link>
                        <router-link to="/log-in" class="button is-light">Авторизация</router-link>
                    </div>
                </div>
            </div>
        </div>
    </nav>
</template>

<script>
export default {
    name: 'Navbar'
}
</script>