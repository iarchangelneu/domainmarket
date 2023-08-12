<template>
    <div class="messenger">
        <div class="left__side" v-if="!isSeller">
            <div class="chat__item" v-for="chat in chats" :key="chat.id"
                @click="newChat(chat.id, chat.seller.user.first_name)">
                <div class="name">
                    <span>{{ chat.seller.user.first_name }}</span>
                    <small>{{ getLastMessageTime(chat) }}</small>
                </div>

                <p class="mb-0">{{ getLastMessageText(chat) }} </p>
            </div>
        </div>
        <div class="left__side" v-if="isSeller">
            <div class="chat__item" v-for="chat in chats" :key="chat.id" @click="newChat(chat.id, chat.buyer.user.email)">
                <div class="name">
                    <span>{{ chat.buyer.user.first_name }}</span>
                    <small>{{ getLastMessageTime(chat) }}</small>
                </div>

                <p class="mb-0">{{ getLastMessageText(chat) }} </p>
            </div>
        </div>

        <div class="right__side">
            <div class="messages">
                <h1>{{ newName }}</h1>

                <div class="chat__block">

                    <div class="message__form" ref="messageContainer" v-if="!isSeller">
                        <div v-for="message in messages" :key="message.time"
                            :class="{ message__to: !message.from_seller, message__from: message.from_seller }">
                            <div class="message">
                                <div class="message__item" :class="{ to: !message.from_seller, from: message.from_seller }">
                                    <p class="mb-0">
                                        {{ message.text }}</p>
                                </div>
                                <div class="time"
                                    :class="{ 'text-right': !message.from_seller, 'text-left': message.from_seller }">
                                    <small>{{ formatTime(message.time) }}</small>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="message__form" ref="messageContainer" v-if="isSeller">
                        <div v-for="message in messages" :key="message.time"
                            :class="{ message__to: message.from_seller, message__from: !message.from_seller }">
                            <div class="message">
                                <div class="message__item" :class="{ to: message.from_seller, from: !message.from_seller }">
                                    <p class="mb-0">
                                        {{ message.text }}</p>
                                </div>
                                <div class="time"
                                    :class="{ 'text-right': message.from_seller, 'text-left': !message.from_seller }">
                                    <small>{{ formatTime(message.time) }}</small>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="sens__msg">
                        <input type="text" v-model="textMessage" placeholder="Введите сообщение..."
                            @keydown.enter.prevent="sendMsg">
                        <img src="@/assets/img/send.svg" alt="" @click="sendMsg" style="cursor: pointer;">
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    props: {
        chatId: Number,
        name: String,
    },
    data() {
        return {
            message: 'Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum Ipsum Ipsum Ipsum Ipsum',
            textMessage: '',
            textMessage2: '',
            newName: this.name,
            newId: this.chatId,
            messages: [],
            isTest: false,
            pathUrl: 'https://d-market.kz',
            msg: [],
            socket: null,
            isSeller: false,
            chats: [],
        }
    },
    computed: {
        lastMessage() {
            return this.chat.messages.length > 0 ? this.chat.messages[this.chat.messages.length - 1] : null;
        },
    },
    methods: {
        getLastMessageTime(chat) {
            const lastMessage = chat.messages && chat.messages.length > 0
                ? chat.messages[chat.messages.length - 1]
                : null;

            return lastMessage && lastMessage.date_time
                ? this.time(lastMessage.date_time)
                : '';
        },
        getLastMessageText(chat) {
            const lastMessage = chat.messages && chat.messages.length > 0
                ? chat.messages[chat.messages.length - 1]
                : null;

            return lastMessage && lastMessage.text ? lastMessage.text : '';
        },
        formatTime(timeString) {
            const timeArray = timeString.split(':');
            const hours = timeArray[0];
            const minutes = timeArray[1];
            const formattedHours = hours.padStart(2, '0');
            const formattedMinutes = minutes.padStart(2, '0');
            return `${formattedHours}:${formattedMinutes}`;
        },
        time(dateTime) {
            const date = new Date(dateTime);
            const hours = date.getHours().toString().padStart(2, '0');
            const minutes = date.getMinutes().toString().padStart(2, '0');
            return `${hours}:${minutes}`;
        },
        getChats() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/messanger/all-chats`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(path)
                .then(response => {
                    this.chats = response.data
                })
                .catch(error => {
                    console.log(error)
                })
        },
        scrollToBottom() {
            const container = this.$refs.messageContainer;
            container.scrollTop = container.scrollHeight;
        },
        sendMsg() {
            if (this.textMessage.trim() === '') return;

            const messageObject = {
                text: this.textMessage,
                from_seller: this.isSeller,
            };

            this.socket.send(JSON.stringify(messageObject));
            this.textMessage = '';
            this.$nextTick(() => {
                this.scrollToBottom();
            });

        },
        onSocketOpen(event) {
            console.log('WebSocket connection opened:', event);

        },
        onSocketMessage(event) {
            ;
            const msg = JSON.parse(event.data);
            if (msg.type === 'chat.message' && msg.messages) {
                // Добавляем только последнее сообщение
                const lastMessage = msg.messages[msg.messages.length - 1];
                this.messages.push(lastMessage);
                this.getChats();
            } else {
                // Обновляем массив messages, заменяя его содержимое на новое
                this.messages = msg;
                this.getChats();
            }

            this.$nextTick(() => {
                this.scrollToBottom();
            });
        },
        onSocketClose(event) {
            console.log('WebSocket connection closed:', event);
        },
        onSocketError(event) {
            console.error('WebSocket error:', event);
        },
        newChat(id, name) {
            this.newId = id
            this.newName = name
            this.socket.close();

            this.startChat()
        },
        startChat() {
            this.socket = new WebSocket(`wss://d-market.kz/ws/messanger/open-chat/${this.newId}`);
            this.socket.addEventListener('open', this.onSocketOpen);
            this.socket.addEventListener('message', this.onSocketMessage);
            this.socket.addEventListener('close', this.onSocketClose);
            this.socket.addEventListener('error', this.onSocketError);

        }
    },
    mounted() {
        this.getChats()
    },
    created() {
        this.startChat()


        const accType = localStorage.getItem('accountType')
        // console.log(accType)
        if (accType == 'buyer-account') {
            this.isSeller = false
        }
        else if (accType == 'seller-account') {
            this.isSeller = true
        }
        else {
            console.log('not authorized')
        }

    },
    beforeDestroy() {
        if (this.socket) {
            this.socket.disconnect();
        }
    },
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