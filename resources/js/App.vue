<script>
export default {
    name: "App",
    data() {
        return {
            token: null
        }
    },

    mounted() {
        this.getToken()
    },

    watch: {
        $route(to, from) {
            this.getToken()
        }
    },

    methods: {

        getToken() {
            this.token = localStorage.getItem('x_xsrf_token')
        },

        logout() {
            axios.post('/logout')
                .then( res => {
                    localStorage.removeItem('x_xsrf_token')
                    this.$router.push({name: 'user.login'})
                })
        }
    }
}
</script>

<template>
    <div>
        <div class="flex justify-between p-8 w-96 mx-auto bg-sky-100">

            <router-link v-if="!token" :to="{ name: 'user.login'}" class="hover:bg-sky-200 ">Войти</router-link>
            <router-link v-if="token" :to="{ name: 'user.index'}" class="hover:bg-sky-200">Пользователи</router-link>
            <router-link v-if="token" :to="{ name: 'user.feed'}" class="hover:bg-sky-200">Лента</router-link>
            <router-link v-if="token" :to="{ name: 'user.personal'}" class="hover:bg-sky-200">Кабинет</router-link>
            <router-link v-if="!token" :to="{ name: 'user.registration'}" class="hover:bg-sky-200">Регистрация</router-link>
            <a v-if="token" @click.prevent="logout" href="#" class="hover:bg-sky-200">Выйти</a>
        </div>

        <router-view></router-view>
    </div>
</template>

<style scoped>

</style>
