<!-- ProcessRules.vue -->

<script>

import ArrayListGrouped from '@shell/components/form/ArrayListGrouped';
import { LabeledInput } from '@components/Form/LabeledInput';
import { RadioGroup } from '@components/Form/Radio';
import { _EDIT } from '@shell/config/query-params';


export default {
    components:{
        ArrayListGrouped,RadioGroup,LabeledInput
    },
  props: {
    processValue: { 
      type: Object,
      required: true
    },
    mode: {
      type: String,
      default: _EDIT
    }
  },
  data() {
    return {
      booleanOptions: [
        { label: 'True', value: true },
        { label: 'False', value: false }
      ]
    };
  }
};
</script>

<template>
  <div class="rule">
      <h3>Match Paths</h3>
      <ArrayListGrouped 
        v-model="processValue.matchPaths" 
        :mode="mode" 
        :add-label="'Add Match Path'"
        :default-add-value="{ path: '', ownerOnly: false, fromSource: [{ path: '' }] }">
        <template #default="props">
          <div class="row mb-20">
            <div class="col span-12">
              <LabeledInput 
                v-model="props.row.value.path" 
                :mode="mode" 
                label="Absolute executable path" />
            </div>
            <div class="col span-12">
              <RadioGroup 
                v-model="props.row.value.ownerOnly" 
                :mode="mode" 
                :options="booleanOptions" />
            </div>
            <div class="col span-12">
              <!-- fromSource should be an array -->
              <ArrayListGrouped
                v-model="props.row.value.fromSource"
                :mode="mode"
                :add-label="'Add Source Path'"
                :default-add-value="{ path: '' }" 
              >
                <template #default="sourceProps">
                  <div class="row mb-20">
                    <div class="col span-12">
                      <LabeledInput
                        v-model="sourceProps.row.value.path"
                        :mode="mode"
                        label="Source Path"
                      />
                    </div>
                  </div>
                </template>
              </ArrayListGrouped>
            </div>
          </div>
        </template>
      </ArrayListGrouped>
  
      <h3>Match Directories</h3>
      <ArrayListGrouped 
        v-model="processValue.matchDirectories" 
        :mode="mode" 
        :add-label="'Add Match Directory'"
        :default-add-value="{ dir: '', recursive: false, ownerOnly: false, fromSource: [] }">
        <template #default="props">
          <div class="row mb-20">
            <div class="col span-12">
              <LabeledInput 
                v-model="props.row.value.dir" 
                :mode="mode" 
                label="Absolute directory path" />
            </div>
            <div class="col span-12">
              <RadioGroup 
                v-model="props.row.value.recursive" 
                :mode="mode" 
                :options="booleanOptions" />
            </div>
            <div class="col span-12">
              <RadioGroup v-model="props.row.value.ownerOnly" :mode="mode" :options="booleanOptions" />
            </div>
            <div class="col span-12">
              <!-- fromSource should now be an array -->
              <ArrayListGrouped 
                v-model="props.row.value.fromSource"
                :mode="mode"
                :add-label="'Add Source'"
                :default-add-value="''">
                <template #default="sourceProps">
                  <LabeledInput
                    v-model="sourceProps.row.value"
                    :mode="mode"
                    label="Source path or process"
                  />
                </template>
              </ArrayListGrouped>
            </div>
          </div>
        </template>
      </ArrayListGrouped>
  
      <h3>Match Patterns</h3>
      <ArrayListGrouped 
        v-model="processValue.matchPatterns" 
        :mode="mode" 
        :add-label="'Add Match Pattern'"
        :default-add-value="{ pattern: '', ownerOnly: false }">
        <template #default="props">
          <div class="row mb-20">
            <div class="col span-12">
              <LabeledInput v-model="props.row.value.pattern" :mode="mode" label="Regex pattern" />
            </div>
            <div class="col span-12">
              <RadioGroup v-model="props.row.value.ownerOnly" :mode="mode" :options="booleanOptions" />
            </div>
          </div>
        </template>
      </ArrayListGrouped>
    </div>
  </template>
