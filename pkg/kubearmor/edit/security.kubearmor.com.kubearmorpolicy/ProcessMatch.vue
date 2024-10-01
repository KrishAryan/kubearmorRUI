
<template>
  <div class="col mb-20">
    <div class="col mb-40">
      <h2>
        Match Path
        <i
          v-clean-tooltip="`MatchPath allows you to define executable paths to be matched.`"
          class="icon icon-info"
        />
      </h2>
      <ArrayListGrouped 
        v-model="value.spec.process.matchPaths" 
        :mode="mode" 
        :add-label="'Add Match Path'"
        :default-add-value="{ path: ''}">
        <template #default="props">
          <div class="col mb-20">
            <div class="col mb-20">
              <LabeledInput 
                v-model="props.row.value.path" 
                :mode="mode" 
                label="Absolute executable path" 
                placeholder="Enter the absolute executable path"
              />
            </div>

            <div class="col mb-20">
              <h3>Owner Only</h3>
              <RadioGroup 
                name="ownerMatchPath"
                v-model="props.row.value.ownerOnly" 
                :mode="mode" 
                :options="options" 
                placeholder="Owner Only"
                v-clean-tooltip="`If enabled, only the owners of the executable(s) will be allowed to execute.`"
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
                    />
                  </div>
                </template>
              </ArrayListGrouped>
            </div>
          </div>
        </template>
      </ArrayListGrouped>
    </div>

    <div class="col mb-40">
      <h2>
        Match Directory
        <i
          v-clean-tooltip="`MatchDirectory allows you to define directories to be matched.`"
          class="icon icon-info"
        />
      </h2>
      <ArrayListGrouped 
        v-model="value.spec.process.matchDirectories" 
        :mode="mode" 
        :add-label="'Add Match Directory'"
        :default-add-value="{ dir: '', fromSource: [] }">
        <template #default="props">
          <div class="col mb-20">
            <div class="col mb-20">
              <LabeledInput 
                v-model="props.row.value.dir" 
                :mode="mode" 
                label="Absolute directory path"
                placeholder="Enter the absolute directory path"
                v-clean-tooltip="`Path to the directory to be matched.`"
              />
            </div>

            <div class="col mb-20">
              <h3>Recursive</h3>
              <RadioGroup 
                name="recursiveMatchDirectories"
                v-model="props.row.value.recursive" 
                :mode="mode" 
                :options="options" 
                placeholder="Recursive"
                v-clean-tooltip="`If enabled, all files and subdirectories in the directory will be recursively matched.`"
              />
            </div>

            <div class="col mb-20">
              <h3>Owner Only</h3>
              <RadioGroup 
                name="ownerMatchDirectories"
                v-model="props.row.value.ownerOnly" 
                :mode="mode" 
                :options="options" 
                placeholder="Owner Only"
                v-clean-tooltip="`If enabled, only the owners of the directory and its contents will have access.`"
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
                      v-clean-tooltip="`The source path for executables that can match within this directory.`"
                    />
                  </div>
                </template>
              </ArrayListGrouped>
            </div>
          </div>
        </template>
      </ArrayListGrouped>
    </div>

    <div class="col mb-40">
      <h2>
        Match Pattern
        <i
          v-clean-tooltip="`MatchPattern allows you to define regex patterns for processes.`"
          class="icon icon-info"
        />
      </h2>
      <ArrayListGrouped 
        v-model="value.spec.process.matchPatterns" 
        :mode="mode" 
        :add-label="'Add Match Pattern'"
        :default-add-value="{ pattern: '' }">
        <template #default="props">
          <div class="col mb-20">
            <div class="col mb-20">
              <LabeledInput 
                v-model="props.row.value.pattern" 
                :mode="mode" 
                label="Regex Pattern"
                placeholder="Enter the regex pattern"
                v-clean-tooltip="`A regular expression pattern to match the process.`"
              />
            </div>

            <div class="col mb-20">
              <h3>Owner Only</h3>
              <RadioGroup 
                name="ownerMatchPattern"
                v-model="props.row.value.ownerOnly" 
                :mode="mode" 
                :options="options" 
                placeholder="Owner Only"
                v-clean-tooltip="`If enabled, only the owners of the processes that match this pattern will be allowed.`"
              />
            </div>
          </div>
        </template>
      </ArrayListGrouped>
    </div>
  </div>
</template>

  <script>
  import ArrayListGrouped from '@shell/components/form/ArrayListGrouped';
  import { LabeledInput } from '@components/Form/LabeledInput';
  import { RadioGroup } from '@components/Form/Radio';
  
  export default {
    components: {
      ArrayListGrouped,
      LabeledInput,
      RadioGroup,
    },
    props: {
      value: Object,
      mode: String,
    },
    data() {
      return {
        ownerOnly: false, 
        options: [
     { label: 'True', value: true },
     { label: 'False', value: false }
   ],
    };
    },
    created() {
      if (!this.value.spec) {
        this.$set(this.value, 'spec', {});
      }
  
      if (!this.value.spec.process) {
        this.$set(this.value.spec, 'process', {
          matchPaths: [],
          matchDirectories: [],
          matchPatterns: []
        });
      }
    }
  };
</script>
