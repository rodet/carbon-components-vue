<template>
  <span
    :class="[
      `cv-tag ${carbonPrefix}--tag`,
      `${carbonPrefix}--tag--${tagKind}`,
      {
        [`${carbonPrefix}--tag--filter`]: isFilter,
        [`${carbonPrefix}--tag--disabled`]: disabled,
      },
    ]"
    role="listitem"
    :title="title"
    @keydown.enter.stop.prevent="$emit('remove')"
    @keydown.space.prevent
    @keyup.space.prevent="$emit('remove')"
  >
    <span :class="`${carbonPrefix}--tag__label`">{{ label }}</span>
    <button
      v-if="isFilter"
      :class="`${carbonPrefix}--tag__close-icon`"
      :aria-label="clearAriaLabel"
      @click.stop.prevent="onRemove"
      :disabled="disabled"
    >
      <Close16 />
    </button>
  </span>
</template>

<script>
import Close16 from '@carbon/icons-vue/es/close/16';
import { carbonPrefixMixin } from '../../mixins';

const tagKinds = [
  'red',
  'magenta',
  'purple',
  'blue',
  'cyan',
  'teal',
  'green',
  'gray',
  'cool-gray',
  'warm-gray',
  'high-contrast',
];

export default {
  name: 'CvTag',
  mixins: [carbonPrefixMixin],
  components: { Close16 },
  props: {
    clearAriaLabel: { type: String, default: 'Clear filter' },
    disabled: Boolean,
    label: { type: String, required: true },
    kind: {
      type: String,
      default: tagKinds[0],
      validator(val) {
        if (val === 'filter' && process.env.NODE_ENV === 'development') {
          console.warn('DEPRECARTED: Prefer props.filter (bool)');
          return true;
        }
        return tagKinds.includes(val);
      },
    },
    filter: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    isFilter() {
      return this.filter || this.kind === 'filter';
    },
    tagKind() {
      return this.kind === 'filter' ? 'high-contrast' : this.kind;
    },
    title() {
      return this.isFilter ? this.clearAriaLabel : null;
    },
  },
  methods: {
    onRemove() {
      if (!this.disabled) {
        this.$emit('remove');
      }
    },
  },
};
</script>
