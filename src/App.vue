<template>
  <div>
    <div class="container column">
    <form class="card card-w30" @submit.prevent="addBlocks">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="blockName">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model="blockText"></textarea>
      </div>

      <button class="btn primary" :disabled="blockText.length < 4">Добавить</button>
    </form>

    <app-dashbord
      :isError="isError"
      :blocks="blocks"
    ></app-dashbord>
  
  </div>

  <app-coments
    
  ></app-coments>
  </div>
</template>

<script>
import AppDashbord from './AppDashbord';
import AppComents from './AppComents';
import axios from 'axios';

export default {
  data() {
    return {
      isError: false,
      blockName: 'title',
      blockText: '',
      blocks: []       
    }


  },
  async mounted () {
    try{
      const {data} = await axios.get('https://third-ui-default-rtdb.europe-west1.firebasedatabase.app/blocks.json')
      this.blocks = Object.values(data)
    } catch (e) {
      this.isError=true
    }
  },
  methods: {
    addBlocks() {
      this.blocks.push({
        blockName: this.blockName,
        blockText: this.blockText
      })

      this.sendToFirebase()

      this.isError = false
      this.blockName = 'title'
      this.blockText = ''
    },
    async sendToFirebase() {
      const data = JSON.stringify({
          blockName: this.blockName,
          blockText: this.blockText
        })
      await axios.post('https://third-ui-default-rtdb.europe-west1.firebasedatabase.app/blocks.json', data)
    }
  },
  components: {AppDashbord, AppComents}
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
