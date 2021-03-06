<template>
  <el-button
    class="mv-button"
    :class="{
      [`mv-button--${computedType || 'standard'}`]: 1,
      [`mv-button--${computedTheme || 'standard'}`]: 1,
      'mv-button--has-text': !!$slots.default,
      'mv-button--has-icon': !!icon || !!iconLeft || !!iconRight
    }"
    :plain="plain"
    :type="computedType"
    :native-type="type"
    :loading="loading"
    v-bind="$attrs"
    v-on="$listeners"
  >
    <mv-icon
      v-if="!loading && (icon || iconLeft)"
      class="mv-button-icon mv-button-icon--left"
      :name="icon || iconLeft"
    />
    <slot></slot>
    <mv-icon
      v-if="iconRight"
      class="mv-button-icon mv-button-icon--right"
      :name="iconRight"
    />
  </el-button>
</template>

<script>
import { Button as ElButton } from "element-ui";
import MvIcon from "./Icon.vue";

export default {
  name: "MvButton",
  components: {
    ElButton,
    MvIcon
  },
  props: {
    icon: String,
    iconLeft: String,
    iconRight: String,
    primary: Boolean,
    secondary: Boolean,
    ghost: Boolean,
    text: Boolean,
    success: Boolean,
    info: Boolean,
    warning: Boolean,
    danger: Boolean,
    type: String,
    loading: Boolean
  },
  computed: {
    plain() {
      return this.secondary || this.ghost;
    },
    computedType() {
      switch (true) {
        case this.text:
          return "text";
        case this.info:
          return "info";
        case this.success:
          return "success";
        case this.warning:
          return "warning";
        case this.danger:
          return "danger";
        case this.primary:
        case this.secondary:
          return "primary";
        default:
          return "";
      }
    },
    computedTheme() {
      switch (true) {
        case this.primary:
          return "primary";
        case this.secondary:
          return "secondary";
        case this.ghost:
          return "ghost";
        default:
          return "";
      }
    }
  }
};
</script>

<style lang="scss">
@import "../theme/_variables.scss";

.mv-button {
  &.el-button {
    font: font-default-bold(1em, 1);
    vertical-align: middle;
  }
  &--has-text {
    .mv-button-icon--left {
      float: left;
      margin-left: -8px;
      margin-right: 8px;
    }
    .mv-button-icon--right {
      float: right;
      margin-left: 8px;
      margin-right: -8px;
    }
    .el-icon-loading {
      float: left;
      margin-left: -8px;
      margin-right: 3px;
    }
  }
}
</style>
