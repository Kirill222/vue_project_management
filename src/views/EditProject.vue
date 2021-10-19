<template>
  <form @submit.prevent="handleSubmit">
        <label>Title:</label>
        <input type="text" v-model="title" required>

        <label>Details:</label>
        <textarea v-model="details" required></textarea>

        <button>Update project</button>

  </form>
</template>

<script>
export default {
    props: ['id'],
    data() {
        return {
            title: '',
            details: '',
            uri: `http://localhost:3000/projects/${this.id}`,
        }
    },
    methods: {
        handleSubmit() {
            let updatedProject = {title: this.title, details: this.details}
            fetch(this.uri, {
                method: 'PATCH',
                headers: {"Content-type": "application/json"},
                body: JSON.stringify(updatedProject)
            })
            .then(() => this.$router.push({name: 'Home'}))
            .catch(err => console.log(err.message))
        }
    },
    mounted() {
        fetch(this.uri)
            .then(res => res.json())
            .then(data => {
                console.log(data)
                this.title = data.title
                this.details = data.details
            })
    }
}
</script>

<style>

</style>