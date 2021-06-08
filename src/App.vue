<template>
<div class="column items-end justify-end contend-end bg-grey-13" style="height: 1000px; width: 100%">
  <div position="botton-right" class="q-pa-md q-gutter-sm self-end " style="height: 400px; max-height: 80vh; max-width: 400px;">
    <q-btn-dropdown
      class="bot_button bottom-right q-ma-md "
      fab
      color="blue-9"
      dropdown-icon="mail"
    > <q-list>
        <q-item-section >
          <q-item-label style="height: 400px; max-height: 400px; max-width: 250px; ">
                <div style="width: 100%; max-width: 400px">
                  <q-scroll-area ref="scrollArea" style="width: 263px; height: 400px;" class="scroll overflow-hidden">
                    <q-chat-message class="message-window"
                      style="width: 243px"
                      v-for="(message, index) in messages"
                      :avatar="('Вы' !== message.name) ? require('./assets/deadpool.jpg') : undefined"
                      :key="index"
                      :text="[message.message]"
                      :sent="'Вы' === message.name"
                    >
                    </q-chat-message>
                  </q-scroll-area>
              </div>
          </q-item-label>
          <q-input  v-model="message"  @keyup.enter="sendMessage" class="mess">
            <template v-slot:append>
              <q-icon name="close" @click="message = ''" class="cursor-pointer" />
            </template>
            <template v-slot:after>
              <q-btn  color='white' bg-color='primary' round dense flat icon="send" @click="sendMessage" />
            </template>
          </q-input>
        </q-item-section>
      </q-list>
    </q-btn-dropdown>
  </div>
</div>
</template>

<script>
import io from 'socket.io-client'
export default {
  name: 'LayoutDefault',

  components: {
  },

  data () {
    return {
      messages: [
        {
          name: 'Бот',
          message: 'Привет,я бот тех поддержк. С чем тебе помочь?'
        }],
      message: '',
      socket: io('http://10.86.29.41:5005')
    }
  },
  methods: {
    sendMessage () {
      this.socket.emit('user_uttered', {
        message: this.message
      })
      this.messages.push({
        name: 'Вы',
        message: this.message
      })
      this.message = ''
    }
  },
  mounted () {
    this.socket.on('connect', () => {
      console.log('connect')
    })
    this.socket.on('bot_uttered', (data) => {
      this.messages.push({
        name: 'Бот',
        message: data.text
      })
    })
    this.socket.on('disconnect', (reason) => {
      console.log(reason)
    })
  }
}
</script>

<style>
.q-menu{
  border-radius: 10px !important;
  padding: 15px;
  margin: 10px 10px 0px 10px !important;
}
.text_box{
  border-radius: 4px 4px 4px 0;
  padding: 8px;
  margin: 7px;
  background: rgb(214, 46, 46);
  color: whitesmoke;
  width: auto;
  cursor: pointer !important;
}
.text{
  display: block;
}
.q-message-text:last-child {
    max-width: 200px;
}
.q-message-text--received{
  background: rgb(145, 36, 36)!important;
}
.q-message-text-content--received{
  color: whitesmoke!important;
}
.q-message-text--received:last-child:before{
  color: rgb(145, 36, 36)!important;
}
.q-message-text--sent{
  background: #616161 !important
}
.q-message-text-content--sent{
  color: whitesmoke!important;
}
.q-position-engine{
  margin-left: 10px !important;
}
.q-field__after{
  padding-left:10px !important;
  padding-right:10px !important;
  background: #1565c0;
}
.q-field__append{
  padding-left:5px !important;
  padding-right:5px !important;
}
.mess{
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  align-items: flex-end;
  align-self: flex-end;
  position: relative;
  background: cornsilk;
}
a {
  color: wheat
}
.scroll{
  -webkit-overflow-scrolling: auto;
}
</style>
