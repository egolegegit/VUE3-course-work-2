<template>
  <div class="container column">
    <ResumeForm @block-added="addBlock" />
    <ResumeView :blocks="blocks" />
  </div>
  <div class="container">
    <AppLoader v-if="loading" />
    <ErrorComments
      v-else-if="commentsError"
      :textError1="textError"
      @close-CommentsError="closeCommentsError"
    />
    <ResumeComments v-else :comments="comments" @load-comments="loadComments" />
  </div>
</template>

<script>
import ResumeForm from './components/ResumeForm'
import ResumeView from './components/ResumeView'
import ResumeComments from './components/ResumeComments'
import AppLoader from './components/AppLoader'
import ErrorComments from './components/ErrorComments'

export default {
  data () {
    return {
      blocks: [],
      comments: [],
      loading: false,
      commentsError: false,
      textError: []
    }
  },
  methods: {
    loadComments () {
      fetch('https://jsonplaceholder.typicode.com/comments?_limit=4')
        .then(async response => {
          const data = await response.json()

          // check for error response
          if (!response.ok) {
            // get error message from body or default to response statusText
            const error =
              (data && data.message) || response.statusText || response.status

            return Promise.reject(error)
          }

          this.loading = true
          this.comments = data
          this.loading = false
        })
        .catch(error => {
          this.commentsError = true
          this.textError.push('Ответ сервера: ' + error)
        })
    },
    addBlock (block) {
      this.blocks.push(block)
    },
    closeCommentsError () {
      this.commentsError = !this.commentsError
      this.textError.length = 0
    }
  },
  components: {
    ResumeForm,
    ResumeView,
    ResumeComments,
    AppLoader,
    ErrorComments
  }
}
</script>
