<template>
  <form @submit.prevent="editProject">
    <label>Başlık</label>
    <input type="text" v-model="title" required>
    <label>Açıklama</label>
    <textarea v-model="detail" required></textarea>
    <button>Proje Ekle</button>
  </form>
</template>

<script>
  export default {
    props: ["id"],
    data(){
      return {
        title: '',
        detail: '',
        uri: "http://localhost:3000/projects/" + this.id,
      }
    },
    mounted() {
      fetch(this.uri)
      .then((res) => res.json())
      .then((data) =>{
        this.title = data.title,
        this.detail = data.details
      })
    },
    methods: {
      editProject(){
        let updateProject = {
          title : this.title,
          details: this.detail
        }
        fetch(this.uri, {
          method: 'PATCH',
          headers: {'Content-Type': 'application/json'},
          body: JSON.stringify(updateProject)
        })
        .then(() => this.$router.push('/'))
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