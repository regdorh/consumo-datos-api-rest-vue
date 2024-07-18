<template>
  <div class="App">
    <div class="container">
      <div class="row justify-content-center mb-3 gap-4">

        <!-- Usuario 1 -->
        <div class="card  col-2 px-0">
          <img :src="firstUser?.picture.large" class="card-img-top" alt="...">
          <div class="card-body">
            <h5 class="card-title">{{ firstUser?.name?.first }} {{ firstUser?.name?.last }}</h5>
            <form
              @submit.prevent="sendMessageHandler(firstUser?.id?.value, firstUserMessage, firstUser?.name?.first, firstUser?.name?.last)"
              id="primera">
              <input class="form-control mb-3" type="color" name="color" id="color" v-model="firstUserColor">
              <input class="form-control mb-3" name="message" id="message" type="text"
                placeholder="Type your message..." v-model="firstUserMessage" />
              <div class="d-grid">
                <button type="submit" class="btn btn-outline-light btn-sm">Send</button>
              </div>
            </form>
          </div>
        </div>

        <!-- Mostrar el chat -->
        <div id="chatBox" class="border border-secondary p-2 col-5 rounded" ref="chatBoxRef">
          <ChatBox :messages="messages" :firstUser="firstUser" :secondUser="secondUser" />
        </div>

        <!-- Usuario 2 -->
        <div class="card col-2 px-0">
          <img :src="secondUser?.picture.large" class="card-img-top" alt="...">
          <div class="card-body">
            <h5 class="card-title">{{ secondUser?.name?.first }} {{ secondUser?.name?.last }}</h5>
            <form
              @submit.prevent="sendMessageHandler(secondUser?.id?.value, secondUserMessage, secondUser?.name?.first, secondUser?.name?.last)"
              id="segunda">
              <input class="form-control mb-3" type="color" name="color" id="color" v-model="secondUserColor">
              <input class="form-control  mb-3" name="secondMessage" id="secondMessage" type="text"
                placeholder="Type your message..." v-model="secondUserMessage">
              <div class="d-grid">
                <button type="submit" class="btn btn-outline-light btn-sm">Send</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { nextTick } from 'vue';
import ChatBox from './components/ChatBox.vue';

export default {
  name: 'App',
  data() {
    return {
      users: [],
      firstUserMessage: '',
      firstUserColor: '#075E54',
      secondUserMessage: '',
      secondUserColor: '#128C7E',
      messages: []
    }
  },
  methods: {
    async buscarUsers() {
      try {
        let response = await axios.get('https://randomuser.me/api/?results=2')
        this.users = response.data.results
      } catch (error) {
        console.log(error)
      }
    },
    sendMessageHandler(id, message, firstName, LastName) {
      const newMessage = {
        userId: id,
        message: message,
        name: firstName,
        last: LastName,
        color: id == this.firstUser.id.value ? this.firstUserColor : this.secondUserColor,
      }
      this.messages.push(newMessage)
      if (id == this.firstUser.id.value) {
        this.firstUserMessage = ''
      } else if (id == this.secondUser.id.value) {
        this.secondUserMessage = ''
      }
      nextTick(() => {
        
        const element = this.$refs.chatBoxRef.lastElementChild;
        console.log(element)

        element && element.scrollIntoView({ behavior: 'smooth', block:'end' })
      })
    }
  },
  computed: {
    firstUser() {
      return this.users[0]
    },
    secondUser() {
      return this.users[1]
    }
  },
  components: {
    ChatBox
  },

  // Lifecycle hooks
  mounted() {
    this.buscarUsers()
  }

}
</script>

<style>
.App {
  min-height: 100vh;
  display: flex;
  place-items: center;
}

.card-img-top {
  width: 218px;
  aspect-ratio: 1/1;
}

.firstUserStyle {
  background-color: #075E54;
  color: white;
  text-align: start;
}

.secondUserStyle {
  background-color: #128C7E;
  color: white;
  text-align: end;
}

.sender {
  font-size: 11px;
}

.time {
  font-size: 10px;
}

#chatBox {
  min-height: 50vh;
  overflow-y: auto;
  max-height: 50vh;
}
.card {
  height: 100% !important;
}
</style>
