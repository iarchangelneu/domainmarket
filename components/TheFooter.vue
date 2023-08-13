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
    padding: 34px 150px;

    @media (max-width: 1500px) {
        padding: 34px 50px;
    }

    @media (max-width: 1024px) {
        padding: 20px;
        margin: 0;
    }

    .links {
        display: flex;
        justify-content: space-between;
        align-items: center;

        @media (max-width: 1024px) {
            flex-direction: column;
            justify-content: unset;
            align-items: flex-start;
            gap: 32px 0;
        }

        img {
            @media (max-width: 1500px) {
                max-width: 200px;
                height: auto;
            }

            @media (max-width: 1300px) {
                max-width: 150px;
                height: auto;
            }

            @media (max-width: 1024px) {
                max-width: 118px;
                height: auto;
            }
        }

        div {
            display: flex;
            gap: 0 80px;

            @media (max-width: 1024px) {
                flex-direction: column;
                gap: 20px;
            }

            a {
                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--cera);
                color: #fff;
                text-decoration: none;

                @media (max-width: 1500px) {
                    font-size: 16px;
                }
            }
        }
    }

    .copyright {
        margin-top: 47px;

        @media (max-width: 1024px) {
            margin-top: 30px;
        }

        p {
            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 150%;
            color: #fff;
            font-family: var(--cera);
            margin-bottom: 0;

            @media (max-width: 1500px) {
                font-size: 16px;
            }

            a {
                color: #fff;
                text-decoration: underline;
            }
        }
    }
}
</style>