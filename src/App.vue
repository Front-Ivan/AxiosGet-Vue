<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="loadComments">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="section">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="info">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model="text"></textarea>
      </div>

      <button class="btn primary" @click="addPart">Добавить</button>
    </form>

    <div class="card card-w70" v-for="item in notes" :key="item.id">
      <h1>
        <span v-if="section === 'title'">{{ text }}</span>
        <span v-if="title.length !== 0">{{ title }}</span>
      </h1>
      <h2>
        <span v-if="section === 'subtitle'">{{ text }}</span>
        <span v-if="subtitle.length !== 0">{{ subtitle }}</span>
      </h2>
      <div class="avatar" v-if="avatar.length !== 0">
        <img :src="avatar">
      </div>
      <div>
        <p v-if="section === 'info'">{{ text }}</p>
        <p v-if="info.length !== 0">{{ info }}</p>
      </div>
      <h3 v-if="notes.length === 0">Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>
  <div class="container">
    <button class="btn primary" @click="loadComments">Загрузить комментарии</button>
    <div class="card white" style="background-color: #2c3e50; margin-top: 15px;">
      <h2>Комментарии</h2>
      <ul class="list" v-if="loaded">
        <li class="list-item" v-for="item in comments" :key="item.id">
          <div>
            <p><strong>{{ item.email }}</strong></p>
            <small>{{ item.body }}</small>
          </div>
        </li>
      </ul>
      <div class="loader" v-else></div>
    </div>
  </div>
</template>

<script>

import axios from 'axios'
// import Swiper from 'swiper'

export default {
  data() {
    return {
      comments: {},
      loaded: false,
      section: 'title',
      text: '',
      // https://www.culture.ru/storage/images/9632c4c5-80e2-5db1-a4f2-5676986c9c41
      notes: [
        {
          id: 1,
          name: 'name',
          title: 'title',
          subtitle: 'subtitle',
          avatar: 'https://ae04.alicdn.com/kf/Ha022ced8496d4f5abc88513597e2325cz/XXXTENTACION-Rap-Star-2018.jpg ',
          info: 'info'
        },
      ],
      title: '',
      subtitle: '',
      avatar: '',
      info: '',
    }
  },
  methods: {
    addPart() {
      if (this.section === 'title') {
        this.title = this.text
      } else if (this.section === 'subtitle') {
        this.subtitle = this.text
      } else if (this.section === 'avatar') {
        this.avatar = this.text
      } else {
        this.info = this.text
      }
      this.text = ''
      console.log(this.avatar)
    },
    async loadComments() {
      try {
        const {data} = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=1000000')
        this.comments = data
        this.loaded = true
      } catch (e) {
        console.log(e)
      }
    },
  },
  mounted() {
    this.loadComments()
  },
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

.white {
  color: #ffffff;
}
</style>
