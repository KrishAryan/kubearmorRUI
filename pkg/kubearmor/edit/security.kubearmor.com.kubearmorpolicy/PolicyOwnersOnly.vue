<script>
import { RadioButton } from '@components/Form/Radio';

export default {
  components: { RadioButton },
  props: {
    value: {
      type: Boolean,
      default: false,
    },
    mode: {
      type: String,
      default: 'edit',
    },
  },
};
</script>

<template>
  <div class="row">
    <div class="col span-4">
      <RadioButton
        v-model="value"
        :mode="mode"
        :options="[{ label: 'True', value: true }, { label: 'False', value: false }]"
        :label="t('kubearmorpolicy.rules.ownerOnly.label')"
      />
    </div>
  </div>
</template>
