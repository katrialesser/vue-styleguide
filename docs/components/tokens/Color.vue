<template>
  <div class="colors">
    <div
      v-for="(prop, index) in tokens"
      :key="index"
      class="color"
      :class="prop.category"
      v-if="prop.type === 'color'">

        <div class="swatch" :style="{ backgroundColor: prop.value }" />
        <h3>{{prop.label}}</h3>
        <span>HEX: {{prop.originalValue}}</span>
        <span>RGB: {{prop.value}}</span>
        <span>Product: {{prop.category.replace(/\d-/g, "")}}</span>
        <span>SCSS: ${{prop.name.replace(/_/g, "-")}}</span>
    </div>
  </div>
</template>

<script>
import designTokens from "@/assets/tokens/tokens.raw.json"
import orderBy from "lodash/orderBy"

/**
 * The color palette comes with 4 main colors for each product, as well as 4 general gray tones.
 * Several additional "extra" colors may be used on occasion outside of the product-specific and gray colors.
 * All color variables names are in the format of prefix `ilws-` and the
 * product abbreviation (`ill`, `ies`, `ima`, `imf`, or `gray`),
 * followed by the descriptor, like `Dark`, `Main`, `Medium`, or `Light`.
 * Extra colors will be few and named appropriately with the `ilws` prefix.
 * To edit the colors, see
 * [/src/tokens/color.yml](https://github.com/viljamis/vue-design-system/blob/master/src/tokens/color.yml).
 */
 /* the name below is the page title - cant change without breaking */
export default {
  name: "Color",
  methods: {
    orderData: function(data) {
      // let byValue = orderBy(data, "value", "asc")
      // let byName = orderBy(data, "name", "asc")
      // let byCategoryAndName = orderBy(byName, "category")

      let byHex = orderBy(data, "originalValue", "asc")
      let byHexAndCategory = orderBy(byHex, "category")

      return byHexAndCategory
    },
  },
  data() {
    return {
      tokens: this.orderData(designTokens.props),
    }
  },
}
</script>

<style lang="scss" scoped>
@import "../../docs.tokens.scss";

/* STYLES
--------------------------------------------- */

.colors {
  display: grid;
  align-content: stretch;
  justify-content: left;
  grid-template-columns:
    calc(20% - #{$space-base}) calc(20% - #{$space-base}) calc(20% - #{$space-base})
    calc(20% - #{$space-base});
  grid-column-gap: $space-base;
  max-width: 1200px;
  width: 100%;
  @media (max-width: 1300px) {
    grid-template-columns: calc(25% - #{$space-base}) calc(25% - #{$space-base}) calc(25% - #{$space-base}) calc(
        25% - #{$space-base}
      );
  }
  @media (max-width: 1100px) {
    grid-template-columns: calc(33.333% - #{$space-base}) calc(33.333% - #{$space-base}) calc(33.333% - #{$space-base});
  }
  @media (max-width: 900px) {
    grid-template-columns: calc(50% - #{$space-base}) calc(50% - #{$space-base});
  }
  @media (max-width: 400px) {
    grid-template-columns: 100%;
  }
}
.swatch {
  border-bottom: 1px solid rgba(0, 0, 0, 0.08);
  height: $space-xx-large;
  margin: -#{$space-small} -#{$space-small} 0;
  width: calc(100% + #{$space-large});
  float: left;
}
h3 {
  @include stack-space($space-x-small);
  text-transform: capitalize;
  line-height: 1.2;
  width: 100%;
  float: left;
}
.color {
  @include reset;
  @include inset-space($space-small);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-bottom: $space-large;
  box-shadow: 0 0 0 1px rgba(63, 63, 68, 0.05), 0 1px 3px 0 rgba(63, 63, 68, 0.15);
  font-size: $font-size-small;
  font-family: $font-family-text;
  color: $ilws-gray-dark;
  border-radius: $border-radius-default;
  overflow: hidden;
  text-align: left;
  float: left;
  width: 100%;
  @media (max-width: 400px) {
    margin-bottom: $space-base;
  }
  &:hover {
    span {
      color: shade($ilws-gray-medium, 40%);
    }
  }
  span {
    margin-bottom: $space-x-small;
    line-height: 1.3;
    color: $ilws-gray-medium;
    font-size: $font-size-small;
    width: 100%;
    float: left;
  }
}
</style>

<docs>
  ```jsx
  <color/>
  ```
</docs>
