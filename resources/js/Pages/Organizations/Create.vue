<template>
  <layout title="Create Organization">
    <h1 class="mb-8 font-bold text-3xl">
      <inertia-link class="text-indigo-light hover:text-indigo-dark" :href="route('organizations')">Organizations</inertia-link>
      <span class="text-indigo-light font-medium">/</span> Create
    </h1>
    <div class="bg-white rounded shadow overflow-hidden max-w-lg">
      <form @submit.prevent="submit">
        <div class="p-8 -mr-6 -mb-8 flex flex-wrap">
          <text-input v-model="form.fields.name" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('name')" label="Name" />
          <text-input v-model="form.fields.email" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('email')" label="Email" />
          <text-input v-model="form.fields.phone" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('phone')" label="Phone" />
          <text-input v-model="form.fields.address" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('address')" label="Address" />
          <text-input v-model="form.fields.city" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('city')" label="City" />
          <text-input v-model="form.fields.region" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('region')" label="Province/State" />
          <select-input v-model="form.fields.country" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('country')" label="Country">
            <option :value="null" />
            <option value="CA">Canada</option>
            <option value="US">United States</option>
          </select-input>
          <text-input v-model="form.fields.postal_code" class="pr-6 pb-8 w-full lg:w-1/2" :error="form.errors.first('postal_code')" label="Postal code" />
        </div>
        <div class="px-8 py-4 bg-grey-lightest border-t border-grey-lighter flex justify-end items-center">
          <loading-button :loading="form.sending" class="btn-indigo" type="submit">Create Organization</loading-button>
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

export default {
  components: {
    InertiaLink,
    Layout,
    LoadingButton,
    SelectInput,
    TextInput,
  },
  data() {
    return {
      form: new Form({
        name: null,
        email: null,
        phone: null,
        address: null,
        city: null,
        region: null,
        country: null,
        postal_code: null,
      }),
    }
  },
  methods: {
    submit() {
      this.form.post({
        url: this.route('organizations.store').url(),
        then: data => Inertia.visit(this.route('organizations.edit', data.id)),
      })
    },
  },
}
</script>
