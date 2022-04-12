<template>
  <div class="form-control">
    <label>
      Label
      <input
        @input="emit('change', $event.target.value)"
        :value="$attrs.value"
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
          :key="sugestion.value"
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
export default {
  name: 'FormInput',
  // inheritAttrs: false,
  props: {
    placeholder: {
      type: String,
      default: 'place',
    },
  },
  data () {
    return {
      sugestions: [
        {
          value: '0123abcdefg',
          part1: '0123',
          part2: 'defg',
          match: 'abc'
        },
        {
          value: 'abcdefg',
          part1: '',
          part2: 'defg',
          match: 'abc'
        },
      ],

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
    }
  }
}
</script>

<style>
  /* .form-control {
    position: relative;
  } */
  .autocomplete {
    background-color: #fff;
    box-shadow: #0008 0 4px 15px -5px;
    display: none;
    max-height: 50vh;
    position: absolute;
    left: 0;
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