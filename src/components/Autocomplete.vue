<template>
  <div style="position:relative" v-bind:class="{'open':openSuggestion}">
    <input class="form-control" type="text" :value="value" @input="updateValue($event.target.value)"
           @keydown.enter='enter'
           @keydown.down='down'
           @keydown.up='up'
    >
    <ul class="dropdown-menu" style="width:100%">
      <li v-for="(suggestion, index) in matches"
          v-bind:class="{'active': isActive(index)}"
          @click="suggestionClick(index)"
      >
        <a href="#">{{ suggestion[anchor].value }} <br/>
          <small>{{ descriptionLabel }}{{ suggestion[description].value }}</small>
        </a>
      </li>
    </ul>
  </div>
</template>

<script type="text/babel">
  export default {

    props: {

      value: {
        type: String,
        required: true,
      },
//      list of objects
      suggestions: {
        type: Array,
        required: true,
      },
//      the key that you would like to search on
      anchor: {
        type: String,
        required: true,
      },
//      the key of supplemental information from the object that will be displayed on line
//      2 of suggestion template
      description: {
        type: String,
        required: true,
      },
//      string label for the second line descripiton to give it context e.g. 'Taxonomy ID: '
      descriptionLabel: {
        type: String,
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
          return obj[this.anchor].value.indexOf(this.value) >= 0;
        });
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
        this.$emit('input', this.matches[this.current][this.anchor].value);

        this.open = false;
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
        this.$emit('input', this.matches[index][this.anchor].value);
        this.open = false;
      },

    },
  };
</script>
