<template>
    <form @submit.prevent="handleSubmit">
    <label >Title:</label>
    <input type="text" v-model="title" required> </input>
    <label >Details:</label>
    <textarea required v-model="details"></textarea>
    <button>Add Project</button>
</form>
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      uri: 'http://localhost:3000/projects/' + this.id,
      title: '',
      details: '',
    }
  },
  mounted() {
    fetch(this.uri)
      .then(res => res.json())
      .then(data => {
        this.title = data.title
        this.details = data.details
      }).catch(err => console.log(err))
  },
  methods : {
    handleSubmit(){
        let project = {
            title : this.title,
            details : this.details,
        }
        fetch(this.uri, {
            method: 'PATCH',
            headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(project)
        }).then(() => {
        this.$router.push('/')
      }).catch(err => console.log(err))
    }
  }
}
</script>


<style scoped>
 form {
    background: white;
    padding: 20px;
    border-radius: 10px;
  }
  label {
    display: block;
    color: #bbb;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 1px;
    margin: 20px 0 10px 0
  }
  input {
    padding: 10px;
    border: 0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    box-sizing: border-box;
  }
  textarea {
    border: 1px solid #ddd;
    padding: 10px;
    width: 100%;
    box-sizing: border-box;
    height: 100px;
  }
  form button {
    display: block;
    margin: 20px auto 0;
    background: #00ce89;
    color: white;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
  }
</style>