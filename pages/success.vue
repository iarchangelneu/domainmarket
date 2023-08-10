<template>
    <div class="success">
        <div class="text-center">
            <h1>ТРАНЗАКЦИЯ ПРОШЛА УСПЕШНО!</h1>
            <NuxtLink to="/">НА ГЛАВНУЮ</NuxtLink>
        </div>
    </div>
</template>
<script setup>
useSeoMeta({
    title: 'Успех | Domain Market',
    ogTitle: 'Успех | Domain Market',
    description: 'Успех | Domain Market',
    ogDescription: 'Успех | Domain Market',
})
</script>
<style lang="scss" scoped>
.success {
    padding: 150px 150px 175px;

    h1 {
        margin-top: 150px;
        font-size: 24px;
        font-style: normal;
        font-weight: 400;
        line-height: 150%;
        font-family: var(--cera);
        color: #fff;
        margin-bottom: 91px;
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