<script>
import { useVuelidate } from '@vuelidate/core'
import { maxLength } from '@vuelidate/validators'

export default {
  setup() {
    return { v$: useVuelidate() }
  },
  data() {
    return {
      profile: {
        avatar: null
      },
      inputs: {
        file: null,
        description: null
      }
    }
  },

  validations() {
    return {
      inputs: {
        file: {
          maxValue: (file) => {
            return file === null || file.size < 512000
          }
        },

        description: {
          maxLength: maxLength(1000)
        }
      }
    }
  },
  methods: {
    async getProfile() {
      const response = await this.$axios.get('/profiles/1')
      const data = response.data
      this.profile.avatar = data.avatar
      this.inputs.description = data.description
    },
    async updateProfile() {
      const valid = await this.v$.$validate()
      if (valid) {
        //todo :)
      }
    },
    handleFileUpload(event) {
      this.inputs.file = event.target.files[0]
    }
  },
  mounted() {
    this.getProfile()
  }
}
</script>

<template>
  <main id="main" class="container-xl my-5 pt-2">
    <h1>My Profile</h1>
    <div class="row row-cols-1 row-cols-md-2 mb-1 mt-4">
      <div class="col-md-4 mb-5">
        <img :src="'/images/' + profile.avatar" class="rounded img-fluid" />
      </div>
      <div class="col-md-4"></div>
    </div>

    <div class="mb-3 mt-4">
      <form novalidate @submit.prevent="updateProfile">
        <div>
          <input
            :class="{ 'is-invalid': v$.inputs.file.$error }"
            name="formFile"
            id="formFile"
            class="form-control"
            type="file"
            accept="image/png,image/gif,image/jpeg"
            @change="handleFileUpload"
          />
          <div class="form-text text-danger" v-if="v$.inputs.file.$error">
            Image size must be less than 500ko
          </div>
          <div class="form-text mb-3" v-else>Photo, avatar or any image.</div>
        </div>
        <div :class="{ error: v$.inputs.description.$errors.maxLength }">
          <label for="description" class="form-label">Description</label>
          <textarea
            v-model="v$.inputs.description.$model"
            name="description"
            id="description"
            class="form-control"
            rows="10"
            >{{ inputs.description }}</textarea
          >
          <div class="input-errors" :class="{ 'text-danger': v$.inputs.description.$error }">
            Text with a maximum of 1000 chars
          </div>
        </div>
        <div class="d-grid gap-2 d-md-flex justify-content-md-end mt-4 buttons-w">
          <button class="btn btn-primary me-md-2" type="submit">Update</button>
        </div>
      </form>
    </div>
  </main>
</template>
