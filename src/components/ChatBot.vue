<template>
    <div class="chatbot">
        <div v-if="messages.length > 0" class="messages">
            <div v-for="(message, index) in messages" :key="index" :class="message.sender">
                <img :src="message.sender === 'user' ? userLogo : botLogo" class="avatar" />
                <div class="message-text" v-html="message.text"></div>
            </div>
            <div v-if="loading" class="loading">Loading...</div>
        </div>
        <input :disabled="loading" v-model="newMessage" @keyup.enter="sendMessage"
            placeholder="Enter your queries here..." />
    </div>
</template>

<script>
import userLogo from '../assets/user.png';
import botLogo from '../assets/machine.png';
import axios from 'axios';

export default {
    data() {
        return {
            messages: [],
            newMessage: '',
            userLogo,
            botLogo,
            loading: false
        }
    },
    methods: {
        async sendMessage() {
            if (this.newMessage.trim() !== '') {
                this.messages.push({ text: this.newMessage, sender: 'user' });
                
                this.loading = true;
                console.log('Sending message:', typeof this.newMessage);
                await axios.post(process.env.VUE_APP_PYTHON_UTILITY_SERVICE_URL + '/generate_response', {
                    input_text: this.newMessage
                }).then((resp)=> {
                    this.newMessage = '';
                    console.log(resp.data)
                    this.messages.push({ text: resp.data, sender: 'bot' });
                    this.loading = false;
                }).catch((error)=> {
                    console.log(error);
                    this.messages.push({ text: 'Unable to process query', sender: 'bot' });
                });
            }
        }
    }
}
</script>

<style scoped>
.chatbot {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 10px;
    background-color: #f9f9f9;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.messages {
    max-height: 400px;
    overflow-y: auto;
    margin-bottom: 10px;
    padding: 10px;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
}

.user,
.bot {
    display: flex;
    margin-bottom: 10px;
}

.user {
    justify-content: flex-end;
}

.bot {
    justify-content: flex-start;
}

.avatar {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    margin: 0 10px;
}

.message-text {
    max-width: 70%;
    padding: 10px;
    border-radius: 10px;
    background-color: #e0e0e0;
    color: #333;
    font-size: 14px;
    line-height: 1.4;
    white-space: pre-wrap;
    /* justify-content: left; */
    text-align: left;
}

.user .message-text {
    background-color: #007bff;
    color: #fff;
}

.loading {
    text-align: start;
    font-style: italic;
    color: #888;
    animation: loadingAnimation 1s infinite;
}

@keyframes loadingAnimation {
    0% {
        opacity: 1;
    }

    50% {
        opacity: 0.5;
    }

    100% {
        opacity: 1;
    }
}

input {
    width: 100%;
    padding: 10px;
    box-sizing: border-box;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 14px;
}
</style>