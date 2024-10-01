
<script>
import { POD } from '@shell/config/types';
import { matching, convert, simplify } from '@shell/utils/selector';
import MatchExpressions from '@shell/components/form/MatchExpressions';
import { Banner } from '@components/Banner';
import ResourceTable from '@shell/components/ResourceTable';

export default {
  components: {
    MatchExpressions,
    Banner,
    ResourceTable,
  },
  props: {
    value: Object,
    mode: String,
    allPods: Array,
    allNamespaces: Array,
    podTableHeaders: Array,
  },
  computed: {
  podSelectorExpressions: {
    get() {
      // Initialize value.spec and value.spec.selector if not defined
      if (!this.value.spec) {
        this.$set(this.value, 'spec', { selector: { matchLabels: {}, matchExpressions: [] } });
      }
      if (!this.value.spec.selector) {
        this.$set(this.value.spec, 'selector', { matchLabels: {}, matchExpressions: [] });
      }
      return convert(this.value.spec.selector.matchLabels || {}, this.value.spec.selector.matchExpressions || []);
    },
    set(podSelectorExpressions) {
      this.value.spec.selector = simplify(podSelectorExpressions);
    },
  },
  matchingPods() {
    if (!this.value.spec || !this.value.spec.selector) {
      return { matched: 0, matches: [], none: true, sample: null, total: 0 };
    }
    
    const allInNamespace = this.allPods.filter(pod => pod.metadata.namespace === this.value.metadata.namespace);
    const match = matching(allInNamespace, this.podSelectorExpressions);

    return {
      matched: match.length || 0,
      matches: match,
      none: match.length === 0,
      sample: match[0]?.nameDisplay,
      total: allInNamespace.length,
    };
  },
}

};
</script>
<template>
    <div>
      <h2>{{ t('networkpolicy.selectors.label') }}</h2>
      <MatchExpressions 
        v-model:value="podSelectorExpressions" 
        :mode="mode" 
        :show-remove="false" 
        :type="POD" 
      />
      <Banner color="success">
        <span v-clean-html="t('networkpolicy.selectors.matchingPods.matchesSome', matchingPods)" />
      </Banner>
  
      <!-- Only show the table if there are matching pods -->
      <ResourceTable 
        v-if="matchingPods.matches.length" 
        :rows="matchingPods.matches" 
        :headers="podTableHeaders" 
        key-field="id" 
        :table-actions="false"
        :schema="podSchema" 
        :groupable="false" 
        :search="false" 
      />
    </div>
  </template>
  