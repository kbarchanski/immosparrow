<template>
  <form @submit="submit">
    <label>Search by</label>
    <select>
        <option 
          v-for="type of searchTypes"
          :value="type.value"
          :key="type.value"
        >
          {{ type.label }}
        </option>
    </select>
    <FormInput v-model="searchTerm" label="Name" placeholder="start typing" />
    <button type="submit">Submit</button>
  </form>
</template>

<script>
import FormInput from './FormInput'

export default {
  name: 'SearchForm',
  components: {
    FormInput,
  },
  data () {
    return {
      searchBy: 'name',
      searchTerm: '',
      searchTypes: [
        {
          value: 'name',
          label: 'Name',
        },
        // {
        //   value: 'status',
        //   label: 'Status',
        // },
        // {
        //   value: 'gender',
        //   label: 'Gender',
        // },
      ]
    }
  },
  methods: {
    submit (e) {
      console.log(e)
    }
  },
}
</script>

<style>

</style>