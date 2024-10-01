<script>
import throttle from 'lodash/throttle';
import CreateEditView from '@shell/mixins/create-edit-view';
import NameNsDescription from '@shell/components/form/NameNsDescription';
import Tab from '@shell/components/Tabbed/Tab';
import Tabbed from '@shell/components/Tabbed';
import CruResource from '@shell/components/CruResource';
import Labels from '@shell/components/form/Labels';
import { NAMESPACE, POD } from '@shell/config/types';
import { allHash } from '@shell/utils/promise';
import { matching } from '@shell/utils/selector';
import KeyValue from '@shell/components/form/KeyValue';
import LabeledSelect from '@shell/components/form/LabeledSelect'; // Import the LabeledSelect component
import FileMatch from './FileMatch.vue';
import ProcessMatch from './ProcessMatch.vue';
import NetworkMatch from './NetworkMatch.vue';
import CapabilitiesMatch from './CapabilitiesMatch.vue';

export default {
  emits: ['input'],
  components: {
    FileMatch,
    ProcessMatch,
    NetworkMatch,
    CapabilitiesMatch,
    CruResource,
    Labels,
    NameNsDescription,
    Tab,
    Tabbed,
    KeyValue,
    LabeledSelect,
  },
  mixins: [CreateEditView],
  async fetch() {
    const hash = await allHash({
      allPods: this.$store.dispatch('cluster/findAll', { type: POD }),
      allNamespaces: this.$store.dispatch('cluster/findAll', { type: NAMESPACE }),
    });

    this.allPods = hash.allPods;
    this.allNamespaces = hash.allNamespaces;
    this.updateMatchingPods();
  },
  data() {
    if (!this.value) {
      this.$set(this, 'value', {});
    }
    if (!this.value.spec) {
      this.$set(this.value, 'spec', {
        selector: { matchLabels: {} },
        action: 'Block',
        process: { matchPaths: [], matchDirectories: [], matchPatterns: [] },
        file: { matchPaths: [], matchDirectories: [], matchPatterns: [] },
        network: { matchProtocols: [] },
        capabilities: { matchCapabilities: [] },
      });
    }
    return {
      actionOptions: ['Allow', 'Audit', 'Block'],
      options: [true, false],
      protocolOption: ['TCP', 'UDP', 'ICMP'],
      POD,
      matchingPods: {
        matched: 0,
        matches: [],
        none: true,
        sample: null,
        total: 0,
      },
      allPods: [],
      allNamespaces: [],
      podTableHeaders: this.$store.getters['type-map/headersFor'](
        this.$store.getters['cluster/schemaFor'](POD)
      ),
      errors: [],
    };
  },

  methods: {
    updateMatchingPods: throttle(function () {
      const allInNamespace = this.allPods.filter((pod) => pod.metadata.namespace === this.value.metadata.namespace);
      const match = matching(allInNamespace, this.value.spec.selector.matchLabels);
      const matched = match.length || 0;
      const sample = match[0]?.nameDisplay;
      this.matchingPods = {
        matched,
        matches: match,
        none: matched === 0,
        sample,
        total: allInNamespace.length,
      };
    }, 250, { leading: true }),
  },
};
</script>

<template>
  <CruResource 
    :done-route="doneRoute" 
    :mode="mode" 
    :resource="value" 
    :validation-passed="true" 
    :errors="errors"
    @error="(e) => (errors = e)" 
    @finish="save" 
    @cancel="done">
    
    <NameNsDescription v-if="!isView" :value="value" :mode="mode" />

    <div class="row mb-40">
      <div class="col span-12">
        <Tabbed :side-tabs="true">
          <Tab name="action" label="Action" :show-header="false" :weight="3">
            <LabeledSelect 
              v-model="value.spec.action"
              :options="actionOptions" 
              label="Action"
              placeholder="Select Action" 
              :mode="mode"
            />
          </Tab>
          <Tab name="selectors" label-key="networkpolicy.selectors.label" :show-header="false" :weight="1">
            <h2>{{ t('networkpolicy.selectors.label') }}
              <i
          v-clean-tooltip="`Specify the match labels to select the pods.`"
          class="icon icon-info"
        />
            </h2>
            <KeyValue
              key="labels"
              v-model:value="value.spec.selector.matchLabels"
              :protected-keys="[]"
              :add-label="t('labels.addLabel')"
              :mode="mode"
              :read-allowed="false"
              :value-can-be-empty="true"                
            />
          </Tab>
          <Tab name="file" label="File" :show-header="false" :weight="3">
            <FileMatch :value="value" :mode="mode" />
          </Tab>

          <Tab name="process" label="Process" :show-header="false" :weight="2">
            <ProcessMatch :value="value" :mode="mode" />
          </Tab>

          <Tab name="network" label="Network" :show-header="false" :weight="2">
            <NetworkMatch :value="value" :mode="mode" />
          </Tab>

          <Tab name="capabilities" label="Capabilities" :show-header="false" :weight="3">
            <CapabilitiesMatch :value="value" :mode="mode" />
          </Tab>
        </Tabbed>
      </div>
    </div>
  </CruResource>
</template>
