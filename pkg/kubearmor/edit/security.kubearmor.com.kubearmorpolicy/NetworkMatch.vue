<template>
  <div class="col mb-40">
    <h2>
      Match Protocols
      <i
        v-clean-tooltip="`Specify the network protocols (TCP, UDP, ICMP) to be matched.`"
        class="icon icon-info"
      />
    </h2>
    <ArrayListGrouped 
      v-model="value.spec.network.matchProtocols" 
      :mode="mode" 
      :add-label="'Add Match Protocol'"
      :default-add-value="{ protocol: '', }">
      <template #default="props">
        <div class="col mb-20">
          <div class="col mb-20">
            <LabeledSelect
              v-model="props.row.value.protocol"
              :mode="mode"
              :options="protocolOption"
              :multiple="false"
              label="Protocol (TCP, UDP, ICMP)"
              placeholder="Select a protocol"
              v-clean-tooltip="`Choose the network protocol to match (e.g., TCP, UDP, or ICMP).`"
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
                    v-clean-tooltip="`Path of the source process that uses this network protocol.`"
                  />
                </div>
              </template>
            </ArrayListGrouped>
          </div>
        </div>
      </template>
    </ArrayListGrouped>
  </div>
</template>

  <script>
  import ArrayListGrouped from '@shell/components/form/ArrayListGrouped';
  import LabeledSelect from '@shell/components/form/LabeledSelect';
  import { LabeledInput } from '@components/Form/LabeledInput';
  
  export default {
    components: {
      ArrayListGrouped,
      LabeledSelect,
      LabeledInput,
    },
    props: {
      value: Object,
      mode: String,
    },
    data() {
      return {
        protocolOption: ['TCP', 'UDP', 'ICMP'],
      };
    },
  };
  </script>
