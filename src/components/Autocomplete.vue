2<template>
  <div style="position:relative" v-bind:class="{'open':openSuggestion}">
    <input class="form-control" type="text" :value="value" @input="updateValue($event.target.value)"
           @keydown.enter = 'enter'
           @keydown.down = 'down'
           @keydown.up = 'up'
    >
    <ul class="dropdown-menu" style="width:100%">
      <li v-for="(suggestion, index) in matches"
          v-bind:class="{'active': isActive(index)}"
          @click="suggestionClick(index)"
      >
        <a href="#">{{ suggestion.taxonLabel.value }} <br/> <small>Taxonomy ID: {{ suggestion.taxid.value }}</small>
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {

    props: {

      value: {
        type: String,
        required: true,
      },

      suggestions: {
        type: Array,
        required: true,
      },

    },

    data() {
      return {
        open: false,
        current: 0,
      };
    },

    computed: {
      // Filtering the suggestion based on the input
      matches() {
        // eslint-disable-next-line
        return this.suggestions.filter((obj) => {
            // eslint-disable-next-line
            return obj.taxonLabel.value.indexOf(this.value) >= 0
            // eslint-disable-next-line
          })
      },

      openSuggestion() {
        return this.selection !== '' &&
          this.matches.length !== 0 &&
          this.open === true;
      },
    },

    methods: {

      updateValue(value) {
        if (this.open === false) {
          this.open = true;
          this.current = 0;
        }
        this.$emit('input', value);
      },

      // When enter pressed on the input
      enter() {
        this.$emit('input', this.matches[this.current].taxonLabel.value);
        this.open = false;
        // eslint-disable-next-line
        console.log('you just chose' + this.matches[this.current].taxonLabel.value);
      },

      // When up pressed while suggestions are open
      up() {
        if (this.current > 0) {
          // eslint-disable-next-line
          this.current--;
        }
      },

      // When up pressed while suggestions are open
      down() {
        if (this.current < this.matches.length - 1) {
          // eslint-disable-next-line
          this.current++;
        }
      },

      // For highlighting element
      isActive(index) {
        return index === this.current;
      },

      // When one of the suggestion is clicked
      suggestionClick(index) {
        this.$emit('input', this.matches[index].taxonLabel.value);
        this.open = false;
      },

    },
  };
</script>
