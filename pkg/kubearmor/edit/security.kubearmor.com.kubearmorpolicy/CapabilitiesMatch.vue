<template>
  <div class="col mb-20">
    <div class="col mb-40">
      <h2>
        Match Capabilities
        <i
          v-clean-tooltip="`Match capabilities allows specifying capabilities that match certain processes.`"
          class="icon icon-info"
        />
      </h2>
      <ArrayListGrouped 
        class="col span-12" 
        v-model="value.spec.capabilities.matchCapabilities" 
        :mode="mode" 
        :add-label="'Add Match Capability'"
        :default-add-value="{ capability: '',}"
      >
        <template #default="props">
          <div class="col mb-20">
            <LabeledInput 
              v-model="props.row.value.capability" 
              :mode="mode" 
              label="Capability Name"
              placeholder="Enter capability name"
              v-clean-tooltip="`The name of the capability that should be matched.`"
            />
          </div>

          <div class="col mb-20">
            <ArrayListGrouped
              v-model="props.row.value.fromSource"
              :mode="mode"
              :add-label="'Add Source Path'"
              :default-add-value="{ path: '' }"
            >
              <template #default="sourceProps">
                <div class="col mb-20">
                  <LabeledInput 
                    v-model="sourceProps.row.value.path" 
                    :mode="mode" 
                    label="Source Path"
                    placeholder="Enter source path"
                    v-clean-tooltip="`The source path for executables that can match the capability.`"
                  />
                </div>
              </template>
            </ArrayListGrouped>
          </div>
        </template>
      </ArrayListGrouped>
    </div>
  </div>
</template>

  <script>
  import ArrayListGrouped from '@shell/components/form/ArrayListGrouped';
  import { LabeledInput } from '@components/Form/LabeledInput';
  import Tab from '@shell/components/Tabbed/Tab';
import Tabbed from '@shell/components/Tabbed';
  export default {
    components: {
      ArrayListGrouped,
      LabeledInput,
      Tab,
      Tabbed,
    },
    props: {
      value: Object,
      mode: String,
    },
    created() {
      if (!this.value.spec) {
        this.$set(this.value, 'spec', {});
      }
      
      if (!this.value.spec.capabilities) {
        this.$set(this.value.spec, 'capabilities', {
          matchCapabilities: []
        });
      }
    },
  };
  </script>
  