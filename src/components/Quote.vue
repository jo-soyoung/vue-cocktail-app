<template>
  <div class="quote" :class="{quoteInSearch}" />
</template>

<script>
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  props: {
    quoteInSearch: {
      type: Boolean,
      default: false
    }
  }
})
</script>

<style lang="scss" scoped>
@import "@/scss/mixins";

.quote {
  &::before {
    content: 'One martini is alright, two is too many, three is not enough';
    display: none;
    position: absolute;
    top: 50%;
    transform: translateY(-52%);
    max-width: 50%;
    font-family: 'Heebo', sans-serif;
    text-align: left;
    font-weight: 700;
    z-index: 30;
  }

  &.quoteInSearch::before {
    content: 'One tequila, two tequila, three tequila, floor';
  }
}

@include tablet {
  .quote {
    &::before {
      display: block;
      left: 3rem;
      font-size: 7rem;
      line-height: 6rem;
    }
  }
}

@include desktop {
  .quote {
      &::before {
        left: 4rem;
        font-size: 10rem;
       line-height: 8rem;
     }
  }
}
</style>
