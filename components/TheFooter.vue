<template>
    <footer v-if="!hideFooterOnPages.includes($route.name)">
        <div class="links">
            <img src="@/assets/img/footerlogo.svg" alt="">
            <div>
                <NuxtLink to="/">личный кабинет</NuxtLink>
                <NuxtLink to="/">пользовательское соглашение</NuxtLink>
                <NuxtLink to="/">политика конфиденциальности</NuxtLink>
            </div>
        </div>

        <div class="copyright">
            <div class="text-center">
                <p>г. Алматы, пр-кт Республики 13, Почтовый индекс A15X3C5 (050013), <a href="tel:+7 775 816 82 48">+7 775
                        816 82 48</a>, БИН 210840016943
                </p>
                <p>Почта поддержки : <a href="mailto:support@designmarket.kz">support@designmarket.kz</a></p>
            </div>
        </div>
    </footer>
</template>
<script>
export default {
    data() {
        return {
            hideFooterOnPages: ['login', 'register'],
        };
    }
}
</script>
<style lang="scss" scoped>
footer {
    border-radius: 20px;
    background: transparent;
    margin: 30px;
    padding: 34px 120px;

    .links {
        display: flex;
        justify-content: space-between;
        align-items: center;

        div {
            display: flex;
            gap: 0 80px;

            a {
                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--cera);
                color: #fff;
                text-decoration: none;
            }
        }
    }

    .copyright {
        margin-top: 47px;

        p {
            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 150%;
            color: #fff;
            font-family: var(--cera);
            margin-bottom: 0;

            a {
                color: #fff;
                text-decoration: underline;
            }
        }
    }
}
</style>