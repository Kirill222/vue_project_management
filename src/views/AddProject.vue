<template>
  <form @submit.prevent="handleSubmit">
        <label>Title:</label>
        <input type="text" v-model="title" required>

        <label>Details:</label>
        <textarea v-model="details" required></textarea>

        <button>Add project</button>

  </form>
</template>

<script>
export default {
    data() {
        return {
            title: '',
            details: '',            
        }
    },
    methods: {
        handleSubmit() {

            let project = {
                title: this.title,      //we do not need to add id. JSON server will handle this
                details: this.details,
                complete: false,
            }

            fetch(' http://localhost:3000/projects', {
                method: 'POST',
                headers: {"Content-Type": "application/json"},
                body: JSON.stringify(project)                
                })
                .then(() => {
                    this.$router.push('/')
                })
                .catch(err => console.log(err.message))
            }
        }
    }

</script>

<style>

form {
    background: #fff;
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
    margin: 20px 0 10px 0;
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
    height: 100px;
    box-sizing: border-box;
}

form button {
    display: block;
    margin: 20px auto 0;
    background: #00ce89;
    color: #fff;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
}

</style>