<script>
import ArrayListGrouped from '@shell/components/form/ArrayListGrouped';
import { LabeledInput } from '@components/Form/LabeledInput';
import { RadioGroup } from '@components/Form/Radio';
import { _EDIT } from '@shell/config/query-params';
import _ from 'lodash';

export default {
  components: {
    ArrayListGrouped, LabeledInput, RadioGroup
  },
  props: {
    value: {
      type: Object,
      default: () => ({
        matchPaths: [],
        matchDirectories: [],
        matchPatterns: []
      })
    },
    mode:{
      type: String,
      default: _EDIT
    }
  },
  data() {
    return {
      booleanOptions: [
        { label: 'True', value: true },
        { label: 'False', value: false }
      ],
      mode: _EDIT // Setting mode here locally
    };
  },
  created() {
    // Ensure all required properties are initialized
    if (!this.value.matchPaths) this.value.matchPaths = [];
    if (!this.value.matchDirectories) this.value.matchDirectories = [];
    if (!this.value.matchPatterns) this.value.matchPatterns = [];
  }
};
</script>

<template>
  <div class="rule">
    <h3>Match Paths</h3>
    <br>
    <ArrayListGrouped
      v-model="value.matchPaths"
      :mode="mode"
      :add-label="'Add Match Path'"
      :default-add-value="{ path: '', ownerOnly: false, fromSource: '' }"
    >
      <template #default="props">
        <div class="row mb-20">
          <div class="col span-12">
            <LabeledInput
              v-model="props.row.value.path"
              :mode="mode"
              label="Absolute executable path"
            />
          </div>
          <div class="col span-12">
            <RadioGroup
              v-model="props.row.value.ownerOnly"
              :mode="mode"
              :options="booleanOptions"
            />
          </div>
          <div class="col span-12">
            <LabeledInput
              v-model="props.row.value.fromSource"
              :mode="mode"
              label="Source path or process"
            />
          </div>
        </div>
      </template>
    </ArrayListGrouped>

    <h3>Match Directories</h3>
    <br>
    <ArrayListGrouped
      v-model="value.matchDirectories"
      :mode="mode"
      :add-label="'Add Match Directory'"
      :default-add-value="{ dir: '', recursive: false, ownerOnly: false, fromSource: '' }"
    >
      <template #default="props">
        <div class="row mb-20">
          <div class="col span-12">
            <LabeledInput
              v-model="props.row.value.dir"
              :mode="mode"
              label="Absolute directory path"
            />
          </div>
          <div class="col span-12">
            <RadioGroup
              v-model="props.row.value.recursive"
              :mode="mode"
              :options="booleanOptions"
            />
          </div>
          <div class="col span-12">
            <RadioGroup
              v-model="props.row.value.ownerOnly"
              :mode="mode"
              :options="booleanOptions"
            />
          </div>
          <div class="col span-12">
            <LabeledInput
              v-model="props.row.value.fromSource"
              :mode="mode"
              label="Source path or process"
            />
          </div>
        </div>
      </template>
    </ArrayListGrouped>

    <h3>Match Patterns</h3>
    <br>
    <ArrayListGrouped
      v-model="value.matchPatterns"
      :mode="mode"
      :add-label="'Add Match Pattern'"
      :default-add-value="{ pattern: '', ownerOnly: false }"
    >
      <template #default="props">
        <div class="row mb-20">
          <div class="col span-12">
            <LabeledInput
              v-model="props.row.value.pattern"
              :mode="mode"
              label="Regex pattern"
            />
          </div>
          <div class="col span-12">
            <RadioGroup
              v-model="props.row.value.ownerOnly"
              :mode="mode"
              :options="booleanOptions"
            />
          </div>
        </div>
      </template>
    </ArrayListGrouped>
  </div>
</template>
