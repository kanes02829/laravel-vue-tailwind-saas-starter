<template>
  <card title="Admin" class="bg-gray-50 dark:bg-gray-800">
    <h3 class="text-lg font-semibold mb-4">
      Tools
    </h3>
    <div class="flex flex-wrap mb-10">
      <a href="/mailcoach">
        <v-button class="mx-1" color="gray" shade="lighter">
          Mailcoach
        </v-button>
      </a>
      <a href="/stats">
        <v-button class="mx-1" color="gray" shade="lighter">
          Stats
        </v-button>
      </a>
      <a href="/horizon">
        <v-button class="mx-1" color="gray" shade="lighter">
          Horizon
        </v-button>
      </a>
    </div>
    <h3 class="text-lg font-semibold mb-4">
      Impersonate User
    </h3>
    <form @submit.prevent="impersonate" @keydown="form.onKeydown($event)">
      <!-- Password -->
      <text-input name="identifier" :form="form" label="Identifier"
                  :required="true" help="User Id or Email"
      />

      <!-- Submit Button -->
      <v-button :loading="loading" type="success" color="nt-blue" class="mt-4 w-full">
        Impersonate User
      </v-button>
    </form>
  </card>
</template>

<script>
import Form from 'vform'
import axios from 'axios'
import FancyLink from '../../components/common/FancyLink'

export default {
  components: { FancyLink },
  middleware: 'admin',
  scrollToTop: false,

  metaInfo () {
    return { title: 'Admin' }
  },

  data: () => ({
    form: new Form({
      identifier: ''
    }),
    loading: false
  }),

  methods: {
    async impersonate () {
      this.loading = true
      this.$store.commit('auth/startImpersonating')
      axios.get('/api/admin/impersonate/' + encodeURI(this.form.identifier)).then(async (response) => {
        // Save the token.
        this.$store.dispatch('auth/saveToken', {
          token: response.data.token,
          remember: false
        })

        this.loading = false

        // Fetch the user.
        await this.$store.dispatch('auth/fetchUser')
        this.$router.push({ name: 'home' })
      }).catch((error) => {
        this.alertError(error.response.data.message)
        this.loading = false
      })

      // this.form.reset()
    }
  }
}
</script>
