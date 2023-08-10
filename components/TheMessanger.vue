<template>
    <div class="messenger">
        <div class="left__side">
            <div class="chat__item">
                <div class="name">
                    <span>alex.ivanov@gmail.com</span>
                    <small>14:47</small>
                </div>

                <p class="mb-0">Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum </p>
            </div>
        </div>

        <div class="right__side">
            <div class="messages">
                <h1>alex.ivanov@gmail.com</h1>

                <div class="chat__block">

                    <div class="message__form" ref="messageContainer">
                        <div class="message__to">
                            <div class="message">
                                <div class="message__item from">
                                    <p class="mb-0">
                                        re</p>
                                </div>
                                <div class="time">
                                    <small>12:04</small>
                                </div>
                            </div>
                        </div>
                        <div class="message__from">
                            <div class="message">
                                <div class="message__item from">
                                    <p class="mb-0">
                                        re
                                    </p>
                                </div>
                                <div class="time">
                                    <small>12:04</small>
                                </div>
                            </div>
                        </div>

                    </div>

                    <div class="sens__msg">
                        <input type="text" placeholder="Введите сообщение..." v-model="newMessage">
                        <img src="@/assets/img/send.svg" alt="" style="cursor: pointer;">
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            newMessage: '',
        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Мессенджер | Domain Market',
    ogTitle: 'Мессенджер | Domain Market',
    description: 'Мессенджер | Domain Market',
    ogDescription: 'Мессенджер | Domain Market',
})
</script>
<style lang="scss" scoped>
.messenger {
    margin-top: 36px;
    display: flex;
    gap: 40px;

    .sens__msg {
        width: 100%;
        display: flex;
        gap: 20px;

        input {
            width: 100%;
            border-radius: 10px;
            border: 2px solid #FFF;
            background: transparent;
            padding: 12px 20px;

            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: 110%;
            color: #fff;
            font-family: var(--cera);

            &::placeholder {
                color: rgba(255, 255, 255, 0.60);
            }
        }
    }

    .right__side {
        width: 100%;

        .messages {
            border-radius: 10px;
            border: 2px solid #FFF;
            padding: 20px 40px;
            width: 100%;

            h1 {
                font-size: 24px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--cera);
                color: #fff;
                margin-bottom: 0;
            }

            .chat__block {
                margin-top: 20px;

                .message__from {
                    display: flex;
                }

                .message__form {
                    margin-top: 45px;
                    height: 468px;
                    overflow: auto;
                }

                .message__form::-webkit-scrollbar {
                    width: 0px;
                    /* в основном для вертикальных полос прокрутки */
                    height: 0px;
                    /* в основном для горизонтальных полос прокрутки */
                }

                .from {
                    border-radius: 30px 30px 30px 0px;
                }

                .to {
                    border-radius: 30px 30px 0 30px;
                }

                .message__from {
                    display: flex;
                }

                .message__to {
                    display: flex;
                    justify-content: flex-end;
                }

                small {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--cera);
                    color: #fff;
                }

                .message__item {
                    padding: 20px;
                    border: 2px solid #fff;
                    max-width: 543px;
                    margin-bottom: 5px;

                    p {
                        font-size: 18px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--cera);
                        color: #fff;
                        white-space: normal;
                    }


                }

            }
        }
    }

    .left__side {
        .chat__item {
            cursor: pointer;
            border-radius: 10px;
            border: 2px solid #FFF;
            padding: 20px;
            margin-bottom: 20px;
            width: 27.083vw;

            &:last-child {
                margin-bottom: 0;
            }

            .name {
                display: flex;
                justify-content: space-between;
                align-items: center;

                span {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--cera);
                    color: #fff;
                }

                small {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--cera);
                    color: #fff;
                }

            }

            p {
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                text-overflow: ellipsis;
                font-family: var(--cera);
                color: rgba(255, 255, 255, 0.70);
                margin-top: 15px;
            }
        }
    }
}
</style>