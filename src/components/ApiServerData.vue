<template>

  <!-- USING FONT AWESOME: explicit style -->
  <!-- <font-awesome-icon :icon="['fas', 'phone']" /> -->

  <!-- USING FONT AWESOME: implicit style (fas is assumed) -->
  <!-- <font-awesome-icon icon="phone" /> -->

  <h1>{{ welcome }}</h1>

  <h2>Profile count: {{ profiles.length }}</h2>

  <ul>
    <li v-for="(profile, index) in profiles" :key="index" align="left">
      {{ index + 1 }}

      <img :src="profile.devImageUrl" alt="" width="50">
      <!-- {{ profile.imgUrl }} -->
      {{ profile._id }}
      {{ profile.devName }}
      {{ profile.devEmail }}
      {{ profile.devWebsiteName }}
    </li>
  </ul>
  <input type="text" v-model.trim="id">{{ id }}
  <button @click="insertDoc">Insert</button>
  <button @click="getDoc">Get</button>
  <button @click="deleteDoc">Delete</button>
  <button @click="updateDoc">Update</button>
  <hr>
  <input type="text" placeholder="First & last name" v-model="formValues.devName">
  <input type="text" placeholder="email" v-model="formValues.devEmail">
  <input type="text" placeholder="imageurls - hover me" title="separate each url with a comma"
    v-model="formValues.devImageUrl">
  <input type="text" placeholder="link to profile" v-model="formValues.devWebsiteName">
</template>

<script>
const api = 'https://t3-backend-server.netlify.app/.netlify/functions/api/'

export default {
  data () {
    return {
      welcome: 'Hello!',
      profiles: [],
      id: '',
      formValues: {
        devName: '',
        devEmail: '',
        devImageUrl: '',
        devWebsiteName: ''
      }
    }
  },
  methods: {
    insertDoc () { // done
      fetch(api, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.formValues)
      })
        .then((response) => response.text())
        .then((data) => {
          console.log(data)
        })
        .catch((err) => {
          if (err) throw err;
        })
    },
    updateDoc () {
      fetch(api + this.id, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.formValues)
      })
        .then((response) => response.text())
        .then((data) => {
          console.log(data)
        })
        .catch((err) => {
          if (err) throw err;
        })
    },
    getDoc () { // done
      fetch(api + this.id, {
        method: 'GET'
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data)
        })
        .catch((err) => {
          if (err) throw err;
        })
    },
    deleteDoc () { // done
      fetch(api + this.id, {
        method: 'DELETE'
      })
        .then((response) => response.text())
        .then((data) => {
          console.log(data)
        })
        .catch((err) => {
          if (err) throw err;
        })

    }
  },
  mounted () {
    fetch(api)
      .then((response) => response.json())
      .then((data) => {
        this.profiles = data
      })
      .catch((err) => {
        if (err) throw err;
      })
  }
}
</script>



<style scoped>
</style>
