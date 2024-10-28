<template>
  <div class="">
    <div class="card " v-show="isShow">
    <div class="card-title bg-pink-500">
      <p class="title-txt">Admin Bot Chat 2.0.1</p>
      <UIcon class="close-icon"  @click="close" name="material-symbols:close-rounded" dynamic />
      <!-- <UIcon class="close-icon" @click="close" name="material-symbols:close-thick" dynamic /> -->
    </div>
    <div class="card-chat" :class="{scrollBar: scroll}" ref="cardChat">
      <div v-for="(item, index) in list" :key="index">
        <!-- Left -->
        <div class="flex justify-end" v-if="item.user === 'me'">
          <div class="bg-blue-500 text-white py-1 px-2 rounded-md my-1 max-w-28">{{ item.text }}</div>
        </div>

         <!-- Right -->
         <div class="flex justify-start" v-else-if="item.user === 'bot'">
          <div class="bg-primary-500 text-white py-1 px-2 rounded-md my-1 max-w-20 ">{{ item.text }}</div>
        </div>
      </div>
    </div>
    <div class="footer">
      <div class="card-message">
        <UInput class="message-inp" type="text" @keyup.enter="sendMessage()" v-model="textMess" variant="none" placeholder="Type a message"/>
        <UButton class="send-btn"  @click="sendMessage()" color="pink">Send</UButton>
      </div>
    </div>
  </div>
  </div>
</template>

<script setup>
  // check Scroll chat
  const scroll = ref(true)

  const cardChat = ref("")
  //close chat
  const isShow = ref(true)
  function close(){
    isShow.value = false
  }

  // create message
  const textMess = ref("")
  const list = ref([])

  function sendMessage(){
    if(textMess.value.trim() !== ""){
      list.value.push({
        user: "me",
        text: textMess.value
      })
      setTimeout(() => {
        scrollToBottom();
      }, 0);
      textMess.value=""
      setTimeout(() => {
          list.value.push({
            user: 'bot',
            text: "I'm bot"
          });
        }, 1000)
        setTimeout(() => {
          scrollToBottom();
        }, 1000);
    }
  }

  function scrollToBottom(){
    if(cardChat.value)
      cardChat.value.scrollTop = cardChat.value.scrollHeight
  }
</script>

<style>
  body{
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .flex{
    margin:0 10px;
  }
  .card{
    width: 350px;
    height: 430px;
    border: 1px solid #ccc;
    overflow: hidden;
    border-radius: 8px;
  }
  .card-title{
    display: flex;
    justify-content: space-between;
    padding: 10px;
    color: white;
  }
  .title-txt{
    display: inline-block;
  }
  .close-icon{
    cursor: pointer;
    margin-top: 3px;
  }
  .card-chat{
    height: 330px;
  }
  .scrollBar{
    overflow-y: scroll;
  }
  .footer{
    display: flex;
    justify-content: center;
    align-items: center;
    border-top: 1px solid #ccc;
  }
  .card-message{
    border: 1px solid #ccc;
    display: inline-block;
    border-radius: 8px;
    margin-top: 10px;
  }
  .message-inp{
    width: 250px;
    display: inline-block;
  }
  .send-btn{
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }
</style>