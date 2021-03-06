<template>
  <div
    :class="{
      'input-group_icon': withIcon.left || withIcon.right,
      'input-group_icon-left': withIcon.left,
      'input-group_icon-right': withIcon.right
    }"
    class="input-group"
  >
    <slot name="left-icon" />
    <component
      :is="type"
      v-bind="$attrs"
      v-on="listeners"
      :value.prop="value"
      :class="{'form-control_sm': small, 'form-control_rounded': rounded}"
      class="form-control"
    >
    </component>
    <slot name="right-icon" />
  </div>
</template>

<script>
export default {
  name: 'AppInput',

  inheritAttrs: false,

  model: {
    prop: 'value',
    event: 'input'
  },

  props: {
    small: {
      type: Boolean,
      default: false
    },
    rounded: {
      type: Boolean,
      default: false
    },
    multiline: {
      type: Boolean,
      default: false
    },
    value: {
      type: String,
      default: ''
    }
  },

  data() {
    return {
      withIcon: {
        left: false,
        right: false
      }
    };
  },

  computed: {
    type() {
      return this.multiline ? 'textarea': 'input';
    },
    listeners() {
      return {
        ...this.$listeners,
        input: ($event) => this.$emit('input', $event.target.value),
        change: ($event) => this.$emit('change', $event.target.value)
      };
    },
  },

  mounted() {
    this.updateWithIcon();
  },

  updated() {
    this.updateWithIcon();
  },

  methods: {
    updateWithIcon() {
      this.withIcon.left = !!this.$slots['left-icon'];
      this.withIcon.right = !!this.$slots['right-icon'];
    },
  }
};
</script>

<style scoped>
.form-control {
  padding: 12px 16px;
  height: 52px;
  border-radius: 8px;
  border: 2px solid var(--blue-light);
  font-family: 'Nunito', sans-serif;
  font-weight: 600;
  font-size: 20px;
  line-height: 28px;
  color: var(--body-color);
  transition: 0.2s all;
  background-color: var(--white);
  outline: none;
  box-shadow: none;
}

.form-control::placeholder {
  font-weight: 400;
  color: var(--blue-2);
}

.form-control:focus {
  border-color: var(--blue);
}

textarea.form-control {
  width: 100%;
  min-height: 211px;
}

.form-control.form-control_rounded {
  border-radius: 26px;
}

.form-control.form-control_sm.form-control_rounded {
  border-radius: 22px;
}

.form-control.form-control_sm {
  padding: 8px 16px;
  height: 44px;
  border-radius: 4px;
}

.input-group {
  position: relative;
}

.input-group .form-control {
  width: 100%;
}

.input-group.input-group_icon .form-control {
  padding-left: 50px;
}

.input-group.input-group_icon .icon {
  position: absolute;
  top: 50%;
  transform: translate(0, -50%);
}

.input-group.input-group_icon.input-group_icon-left .icon:first-child {
  left: 16px;
}

.input-group.input-group_icon.input-group_icon-right .icon:last-child {
  right: 16px;
}
</style>
