<script>
import { useVuelidate } from '@vuelidate/core'
//keep going with https://vuelidate-next.netlify.app/#alternative-syntax-composition-api
//https://vuelidate-next.netlify.app/examples.html#form-submission
export default {
  setup() {
    return { v$: useVuelidate() }
  },
  data() {
    return {
      file: '',
      description: ''
    }
  },
  validations() {
    return {
      // file:
      // description:
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
      <form>
        <input
          class="form-control"
          type="file"
          id="formFile"
          accept="image/png,image/gif,image/jpeg"
        />
        <div class="form-text text-danger">Image size must be less than 500ko</div>
        <div class="form-text mb-3">Photo, avatar or any image.</div>

        <label for="description" class="form-label">Description</label>
        <textarea name="description" id="description" class="form-control" rows="10"></textarea>
        <div class="form-text">Text with a maximum of 1000 chars.</div>

        <div class="d-grid gap-2 d-md-flex justify-content-md-end mt-4">
          <button class="btn btn-primary me-md-2" type="submit">Update</button>
        </div>
      </form>
    </div>
  </main>
</template>
