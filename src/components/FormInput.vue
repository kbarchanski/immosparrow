<template>
  <div class="form-control">
    <label>
      {{ label }}
      <input
        @input="$emit('input', $event.target.value)"
        @blur="onBlur"
        @focus="openAutocomplete"
        :value="value"
        :placeholder="placeholder"
        type="text"
        ref="input"
      >
      <div 
        ref="autocomplete"
        class="autocomplete"
        :class="{ open: open }"
        :style="autocompletePos"
      >
        <div 
          v-for="sugestion of sugestions"
          @click="submit(sugestion, $event)"
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
      autocompletePos: {},
      sugestions: [],
      open: false,
      noRequest: false
    }
  },
  mounted () {
    this.setAutocompletePos()
  },
  methods: {
    onBlur () {
      setTimeout(()=>this.closeAutocomplete(), 250)
    },
    setAutocompletePos () {
      const {left, top, width, height} = this.$refs.input.getBoundingClientRect()
      this.autocompletePos = {
        left: left + 'px',
        top: top + height + 'px',
        width: width + 'px',
      }
    },
    closeAutocomplete () {
      this.open = false
    },
    openAutocomplete () {
      if (this.sugestions.length) {
        this.setAutocompletePos()
        this.open = true
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
        this.openAutocomplete()

      } catch (error) {
        // TODO: handle errors; display message on 404?
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
    },
    submit (item) {
      this.noRequest = true
      this.$emit('input', item.value)
      this.$emit('submit')
    },
  },
  watch: {
    value: debounce(function (newValue) {
      if(this.noRequest) {
        this.noRequest = false
      } else {
        this.searchFor(newValue)
      }
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
  .autocomplete.open {
    display: block;
  }
  .autocomplete-item {
    border-bottom: 1px solid #000a;
    padding: .5em 1em;
    cursor: pointer;
  }
  .autocomplete-item:hover {
    background-color: #eee;
  }
  .autocomplete-match {
    font-weight: 600;
  }
</style>