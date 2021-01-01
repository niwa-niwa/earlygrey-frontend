<template>
  <button @click="handleLogin">
    Googleでログイン
  </button>
</template>

<script>
import firebase from '@/plugins/firebase'
const axios = require('axios')
const url = 'http://localhost'

export default {
  async mounted () {
    try {
      const { user } = await firebase.auth().getRedirectResult()
      if (user) {
        const idToken = await user.getIdToken(true)
        const { data } = await axios.post(url + '/api/auth/', { idToken }).then((response) => {
          // eslint-disable-next-line
          console.log(response)
        })
        axios.setToken(data.token, 'Bearer')
      }
    } catch (e) {
      // eslint-disable-next-line
      console.log('エラーが発生しました。')
      // eslint-disable-next-line
      console.log(e)
    }
  },
  methods: {
    handleLogin () {
      const provider = new firebase.auth.GoogleAuthProvider()
      firebase.auth().signInWithRedirect(provider)
    }
  }
}
</script>
