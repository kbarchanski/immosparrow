<template>
  <div class="form-control">
    <label>
      {{ label }}
      <input
        @input="$emit('input', $event.target.value)"
        :value="value"
        :placeholder="placeholder"
        type="text"
        ref="input"
      >
      <div 
        ref="autocomplete"
        class="autocomplete active"
        :style="autocompletePos"
      >
        <div 
          v-for="sugestion of sugestions"
          :key="sugestion.id"
          class="autocomplete-item"
        >
            <span v-text="sugestion.part1" />
            <span v-text="sugestion.match" class="autocomplete-match"/>
            <span v-text="sugestion.part2" />
        </div>
      </div>
    </label>
  </div>
</template>

<script>
import axios from 'axios'
import { debounce } from '../services/utils'

export default {
  name: 'FormInput',
  props: {
    label: {
      type: String,
      default: 'Label',
    },
    placeholder: {
      type: String,
      default: '',
    },
    value: {
      type: String,
      default: '',
    },
  },
  data () {
    return {
      sugestions: [],
      autocompletePos: {}
    }
  },
  mounted () {
    this.setAutocompletePos()
  },
  methods: {
    setAutocompletePos () {
      const {left, top, width, height} = this.$refs.input.getBoundingClientRect()
      this.autocompletePos = {
        left: left + 'px',
        top: top + height + 'px',
        width: width + 'px',
      }
    },
    async searchFor (value) {
      let response
      try {
        response = await axios
          .get('https://rickandmortyapi.com/api/character',{
            params: {
              name: value
            }
          })

        this.sugestions = this.mapToSugestions(response.data.results, value)

      } catch (error) {
        console.error(error)
      }
    },
    mapToSugestions (results, searchValue) {
      const sug = results.map(result => {
        const match = result.name.match(RegExp(searchValue, 'i'))

        return {
          value: result.name,
          part1: result.name.slice(0,match.index),
          part2: result.name.slice(match.index + match[0]?.length),
          match: match[0],
          id: result.id,
        }
      })

      return sug
    }
  },
  watch: {
    value: debounce(function (newValue) {
      console.log(newValue)
      this.searchFor(newValue)
    }, 200)
  },
}
</script>

<style>
  .autocomplete {
    background-color: #fff;
    box-shadow: #0008 0 4px 15px -5px;
    display: none;
    max-height: 50vh;
    position: absolute;
    left: 0;
    overflow: scroll;
  }
  .autocomplete.active {
    display: block;
  }
  .autocomplete-item {
    border-bottom: 1px solid #000a;
    padding: .5em 1em;
  }
  .autocomplete-match {
    font-weight: 600;
  }
</style>