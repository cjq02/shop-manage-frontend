<template>
  <el-dialog
    v-model="visible"
    title="Chat Box"
    width="800px"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
    append-to-body
  >
    <div class="chat-container">
      <div v-for="(item, itemIndex) in messageList" :key="itemIndex" class="msg-item">
        <div class="chat-user">
          <span class="user-name">{{ item.sender }}</span>
          <span class="send-time">{{ $filters.dateFormat(item.sendTs) }}</span>
        </div>
        <div class="msg-content">
          {{ item.content }}
        </div>
      </div>
    </div>
    <template #footer>
      <div class="footer-wrapper">
        <el-input v-model="message" placeholder="Please input message"></el-input>
        <el-button type="success" class="ml10" @click="sendMessage">Send</el-button>
      </div>
    </template>
  </el-dialog>
</template>
<script lang="ts" setup>
import 'https://cdnjs.cloudflare.com/ajax/libs/sockjs-client/1.1.4/sockjs.min.js'
import 'https://cdnjs.cloudflare.com/ajax/libs/stomp.js/2.3.3/stomp.min.js'

const userStore = useUserStore()

const visible = ref(false)
const stompClient = ref<any>(null)
const message = ref('')
const messageList = ref<any>([])

function open() {
  visible.value = true
  connect()
}

function connect() {
  const socket = new SockJS(`http://localhost:10203${apiUtils.API_PREFIX}/ws`)
  stompClient.value = Stomp.over(socket)
  stompClient.value.connect({}, onConnected, onError)
}

function onConnected() {
  // Subscribe to the Public Topic
  stompClient.value.subscribe('/topic/public', onMessageReceived)
  // Tell your username to the server
  stompClient.value.send(
    '/app/chat.addUser',
    {},
    JSON.stringify({ sender: userStore.userInfo.realName, type: Code.CHAT_MESSAGE_TYPE.JOIN }),
  )
}

function onError(error) {
  console.log('onError', error)
  /* showConnectError.value = true*/
}

function sendMessage() {
  const messageContent = message.value.trim()

  if (messageContent && stompClient.value) {
    const chatMessage = {
      content: message.value,
      sender: userStore.userInfo.realName,
      type: Code.CHAT_MESSAGE_TYPE.CHAT,
    }

    stompClient.value.send('/app/chat.sendMessage', {}, JSON.stringify(chatMessage))
    message.value = ''
  }
}

function onMessageReceived(payload) {
  const messageBody = JSON.parse(payload.body)

  console.log('onMessageReceived', messageBody)
  if (messageBody.type === Code.CHAT_MESSAGE_TYPE.CHAT) {
    messageList.value.push(messageBody)
  }
}

defineExpose({ open })
</script>
<style lang="scss" scoped>
.chat-container {
  height: 400px;
  .msg-item {
    .chat-user {
      .user-name {
        font-size: 16px;
        font-weight: bold;
      }
      .send-time {
        margin-left: 10px;
        color: darkgray;
      }
    }
    .msg-content {
      min-height: 50px;
      line-height: 50px;
    }
  }
}
.footer-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
