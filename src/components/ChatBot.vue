<template>
    <div class="chatbot">
        <div v-if="messages.length > 0" id="messages">
            <div v-for="(message, index) in messages" :key="index" :class="message.sender">
                <img :src="message.sender === 'user' ? userLogo : botLogo"
                    :alt="message.sender === 'user' ? 'User icons created by Freepik - Flaticon' : 'Robot icons created by Hilmy Abiyyu A. - Flaticon'"
                    class="avatar" />
                <div class="message-text" v-html="message.text"></div>
            </div>
            <div v-if="loading" class="loading">Loading...</div>
        </div>
        <div v-else id="greeting">
            <div>
                <img :src="botLogo" alt="Robot icons created by Hilmy Abiyyu A. - Flaticon" class="avatar"
                    style="transform: scale(2);" />
                <div style="padding: 20px;">
                    <h2>Hi! How May I Help?</h2>
                </div>
                <!-- <div class="message-text">Hello! I am a chatbot. How can I help you today?</div> -->
            </div>
        </div>
        <div id="input" :style="inputStyle">
            <input :disabled="loading" v-model="newMessage" @keyup.enter="sendMessage"
                placeholder="Enter your queries here..." />
        </div>

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
            loading: false,
            inputStyle: null
        }
    },
    methods: {
        async sendMessage() {
            const localNewMessage = this.newMessage;
            if (localNewMessage.trim() !== '') {
                this.messages.push({ text: localNewMessage, sender: 'user' });
                this.newMessage = '';
                this.loading = true;
                this.inputStyle = { position: 'absolute', bottom: '0', width: '80%', marginBottom: '20px' };
                console.log('Sending message:', typeof localNewMessage);
                await axios.post(process.env.VUE_APP_PYTHON_UTILITY_SERVICE_URL + 'generate_response', {
                    input_text: localNewMessage
                }).then((resp) => {

                    console.log(resp.data)
                    this.messages.push({ text: resp.data.response, sender: 'bot' });
                    this.loading = false;
                }).catch((error) => {
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
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    /* display: flex; */
    /* justify-self: center;
    align-items: center; */

    /* max-width: 600px;
    min-width: 500px; */
    /* margin: 0 auto;
    padding: 20px; */

    /* border: 1px solid #ccc; */
    /* border-radius: 10px; */
    /* background-color: #f9f9f9; */
    /* box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); */
}

#messages {
    position: absolute;
    top: 0;
    margin-top: 20px;
    overflow-y: auto;
    max-width: 80%;
    min-width: 80%;
    max-height: 85vh;

    /* max-height: 400px;
    overflow-y: auto;
    margin-bottom: 10px;
    padding: 10px;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1); */
}

@keyframes typing {
    from {
        width: 0
    }

    to {
        width: 100%
    }
}

/* The typewriter cursor effect */
@keyframes blink-caret {

    from,
    to {
        border-color: transparent
    }

    50% {
        border-color: rgb(0, 166, 255);
    }
}


#greeting div h1 {
    overflow: hidden;
    /* Ensures the content is not revealed until the animation */
    border-right: .15em solid rgb(0, 166, 255);
    /* The typwriter cursor */
    white-space: nowrap;
    /* Keeps the content on a single line */
    margin: 0 auto;
    /* Gives that scrolling effect as the typing happens */
    letter-spacing: .15em;
    /* Adjust as needed */
    animation:
        typing 2s steps(29, end),
        blink-caret .75s step-end infinite;
}

#input {
    /* display: flex; */
    animation: typing 1.5s;
    width: 100%;

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