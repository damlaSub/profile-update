<script>
import { useVuelidate } from '@vuelidate/core'
import { maxLength } from '@vuelidate/validators'

export default {
  setup() {
    return { v$: useVuelidate() }
  },
  data() {
    return {
      form: {
        file: '',
        description: ''
      }
    }
  },

  validations() {
    return {
      form: {
        file: {
          maxValue: (file) => {
            return file.size > 512000 ? false : true
          }
        },

        description: {
          maxLength: maxLength(1000)
        }
      }
    }
  },
  methods: {
    handleFileUpload(event) {
      this.form.file = event.target.files[0]
    },
    submitForm(event) {
      this.v$.$validate()
    }
  }
}
</script>

<template>
  <main id="main" class="container-xl my-5 pt-2">
    <h1>My Profile</h1>
    <div class="row row-cols-1 row-cols-md-2 mb-1 mt-4">
      <div class="col-md-4 mb-5">
        <img
          src="/images/1d53713d-6b20-4398-a1a8-a598ce19b1bdboule_noire.jpg"
          class="rounded img-fluid"
        />
      </div>
      <div class="col-md-4"></div>
    </div>

    <div class="mb-3 mt-4">
      <form @submit.prevent="submitForm">
        <div>
          <input
            v-on:change="handleFileUpload"
            name="formFile"
            id="formFile"
            class="form-control"
            type="file"
            accept="image/png,image/gif,image/jpeg"
          />
          <div class="input-errors" v-for="(error, index) of v$.form.file.$errors">
            <div class="form-text text-danger">Image size must be less than 500ko</div>
          </div>
          <div class="form-text mb-3">Photo, avatar or any image.</div>
        </div>
        <div :class="{ error: v$.form.description.$errors.maxLength }">
          <label for="description" class="form-label">Description</label>
          <textarea
            v-model="v$.form.description.$model"
            name="description"
            id="description"
            class="form-control"
            rows="10"
          ></textarea>
          <div class="input-errors" v-for="(error, index) of v$.form.description.$errors">
            <div class="form-text error-msg text-danger">Text with a maximum of 1000 chars .</div>
          </div>
        </div>
        <div class="d-grid gap-2 d-md-flex justify-content-md-end mt-4 buttons-w">
          <button class="btn btn-primary me-md-2" type="submit" :disabled="v$.form.invalid">
            Update
          </button>
        </div>
      </form>
    </div>
  </main>
</template>
