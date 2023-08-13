<template>
    <div class="failure">
        <div class="text-center">
            <h1>ПРИ ПРОВЕДЕНИИ ТРАНЗАКЦИИ ПРОИЗОШЛА ОШИБКА<br>
                ПОВТОРИТЕ ЗАПРОС ПОЗДНЕЕ</h1>
            <NuxtLink to="/">НА ГЛАВНУЮ</NuxtLink>
        </div>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            pathUrl: 'https://d-market.kz',
        }
    },
    methods: {
        sendRequest(reference) {
            const token = this.getAuthorizationCookie();
            const path = `${this.pathUrl}/api/money/failure/${reference}`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    console.log(response)
                })
                .catch(error => {
                    console.log(error);
                });
        },
    },
    mounted() {
        const url = window.location.href;
        const match = url.match(/order_pay_(\d+)/);

        if (match) {
            this.extractedValue = match[0];
            console.log(this.extractedValue);

            this.sendRequest(match[0])
        }
    },

}
</script >
<script setup>
useSeoMeta({
    title: 'Неудача | Domain Market',
    ogTitle: 'Неудача | Domain Market',
    description: 'Неудача | Domain Market',
    ogDescription: 'Неудача | Domain Market',
})
</script>
<style lang="scss" scoped>
.failure {
    padding: 150px 150px 175px;

    @media (max-width: 1440px) {
        padding: 150px 50px 50px;
    }

    @media (max-width: 1024px) {
        padding: 120px 20px 50px;
    }

    h1 {
        margin-top: 150px;
        font-size: 24px;
        font-style: normal;
        font-weight: 400;
        line-height: 150%;
        font-family: var(--cera);
        color: #fff;
        margin-bottom: 91px;

        @media (max-width: 1024px) {
            font-size: 16px;
        }
    }

    a {
        text-decoration: none;
        border-radius: 10px;
        border: 2px solid #FFF;
        padding: 12px 71px;

        font-size: 18px;
        font-style: normal;
        font-weight: 400;
        line-height: 110%;
        font-family: var(--cera);
        color: #fff;
        transition: all .3s ease;

        &:hover {
            color: #000;
            background: #fff;
        }
    }
}
</style>