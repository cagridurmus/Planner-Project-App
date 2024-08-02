<template>
  <form @submit.prevent="addProject">
    <label>Başlık</label>
    <input type="text" v-model="title">
    <label>Açıklama</label>
    <textarea v-model="detail"></textarea>
    <button>Proje Ekle</button>
  </form>
</template>

<script>
export default {
  data(){
    return{
      title: '',
      detail: '',
      uri: 'http://localhost:3000/projects'
    }
  },
  methods: {
    addProject(){
      let project = {
        title: this.title,
        detail: this.detail,
        completed : false
      }
      fetch(this.uri, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(project)
      })
      .then(() => this.$router.push("/"))
      .catch((err) => alert(err.message))
    }
  },
}
</script>

<style>
form{
  background: #fff;
  padding: 20px;
  border-radius: 10px;
}

label{
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}

input{
  padding: 10px;
  border: 0;
  border-bottom: 2px solid #ddd;
  width: 100%;
  box-sizing: border-box;
}

textarea{
  padding: 10px;
  border-bottom: 2px solid #ddd;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
}

form button{
  display: block;
  margin: 20px auto 0;
  background: #76dd78;
  border: 0;
  color: #fff;
  padding: 10px;
  border-radius: 7px;
  font-size: 16px;
}

</style>