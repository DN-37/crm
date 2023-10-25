<template>
    <div class="container">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Авторизация</h1>

                <form>
                    <div class="field">
                        <label>Электронная почта</label>
                        <div class="control">
                            <input type="email" name="email" class="input">
                        </div>
                    </div>

                    <div class="field">
                        <label>Пароль</label>
                        <div class="control">
                            <input type="password" name="password" class="input">
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
export default {
    name: 'LogIn'
}
</script>