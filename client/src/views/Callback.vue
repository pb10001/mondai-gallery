<template>
  <div>
    <form @submit.prevent="post">
      <b-input v-model="title" placeholder="タイトル"></b-input>
      <b-input v-model="content" type="textarea" placeholder="内容"></b-input>
      <button class="button is-primary is-outlined">投稿</button>
    </form>
  </div>
</template>
<script>
export default {
  data () {
    return {
      title: '',
      content: ''
    }
  },
  mounted () {
    if (this.$store.state.tumblrToken.token) return
    let data = {
      token: this.$route.query.token,
      secret: this.$route.query.secret
    }
    this.$store.commit('setTumblrToken', data)
  },
  methods: {
    post () {
      this.$http.post('/api/post', {
        token: this.$store.state.tumblrToken.token,
        secret: this.$store.state.tumblrToken.secret,
        title: this.title,
        content: this.content
      })
        .then(res => {
            this.$toast.open({
              'type': 'is-success',
              'message': '送信しました。'
            })
            this.$router.push('/mypage')
          })
        .catch(err => {
          if (err) {
            this.$toast.open({
              'type': 'is-danger',
              'message': '送信エラーです。'
            })
          }
        })
    }
  }
}
</script>
