<template>
  <layout :title="`${form.fields.first_name} ${form.fields.last_name}`">
    <h1 class="mb-8 font-bold text-3xl">
      <inertia-link class="text-indigo-light hover:text-indigo-dark" :href="route('users')">Users</inertia-link>
      <span class="text-indigo-light font-medium">/</span>
      {{ form.fields.first_name }} {{ form.fields.last_name }}
    </h1>
    <trashed-message v-if="user.deleted_at" class="mb-6" @restore="restore">
      This user has been deleted.
    </trashed-message>
    <div class="bg-white rounded shadow overflow-hidden max-w-lg">
      <form @submit.prevent="submit">
        <div class="p-8 -mr-6 -mb-8 flex flex-wrap">
          <text-input v-model="form.fields.first_name" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('first_name')" label="First name" />
          <text-input v-model="form.fields.last_name" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('last_name')" label="Last name" />
          <text-input v-model="form.fields.email" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('email')" label="Email" />
          <text-input v-model="form.fields.password" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('password')" type="password" autocomplete="new-password" label="Password" />
          <select-input v-model="form.fields.owner" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('owner')" label="Owner">
            <option :value="true">Yes</option>
            <option :value="false">No</option>
          </select-input>
        </div>
        <div class="px-8 py-4 bg-grey-lightest border-t border-grey-lighter flex items-center">
          <button v-if="!user.deleted_at" class="text-red hover:underline" tabindex="-1" type="button" @click="destroy">Delete User</button>
          <loading-button :loading="form.sending" class="btn-indigo ml-auto" type="submit">Update User</loading-button>
        </div>
      </form>
    </div>
  </layout>
</template>

<script>
import { Inertia, InertiaLink } from 'inertia-vue'
import Form from '@/Utils/Form'
import Layout from '@/Shared/Layout'
import LoadingButton from '@/Shared/LoadingButton'
import SelectInput from '@/Shared/SelectInput'
import TextInput from '@/Shared/TextInput'
import TrashedMessage from '@/Shared/TrashedMessage'

export default {
  components: {
    InertiaLink,
    Layout,
    LoadingButton,
    SelectInput,
    TextInput,
    TrashedMessage,
  },
  props: {
    user: Object,
    organizations: Array,
  },
  data() {
    return {
      form: new Form({
        first_name: this.user.first_name,
        last_name: this.user.last_name,
        email: this.user.email,
        password: this.user.password,
        owner: this.user.owner,
      }),
    }
  },
  methods: {
    submit() {
      this.form.put({
        url: this.route('users.update', this.user.id).url(),
        then: () => Inertia.visit(this.route('users')),
      })
    },
    destroy() {
      if (confirm('Are you sure you want to delete this user?')) {
        this.form.delete({
          url: this.route('users.destroy', this.user.id).url(),
          then: () => Inertia.replace(this.route('users.edit', this.user.id).url()),
        })
      }
    },
    restore() {
      if (confirm('Are you sure you want to restore this user?')) {
        this.form.put({
          url: this.route('users.restore', this.user.id).url(),
          then: () => Inertia.replace(this.route('users.edit', this.user.id).url()),
        })
      }
    },
  },
}
</script>
