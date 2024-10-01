<template>
  <div class="col mb-20">
    <div class="col mb-40">
      <h2>
        Match Paths
        <i
          v-clean-tooltip="`Match file paths for security policies.`"
          class="icon icon-info"
        />
      </h2>
      <ArrayListGrouped 
        v-model="value.spec.file.matchPaths" 
        :mode="mode" 
        :add-label="'Add Match Path'"
        :default-add-value="{ path: '', }">
        <template #default="props">
          <div class="col mb-20">
            <div class="col mb-20">
              <LabeledInput 
                v-model="props.row.value.path" 
                :mode="mode" 
                label="Absolute file path" 
                placeholder="Enter the absolute file path"
                v-clean-tooltip="`Path to the file to be matched.`"
              />
            </div>

            <div class="col mb-20">
              <RadioGroup 
                v-model="props.row.value.ownerOnly" 
                :mode="mode" 
                :options="options" 
                label="Owner Only"
                placeholder="Owner Only"
                name="matchPathsOwnerOnly"
                v-clean-tooltip="`If enabled, only the file owner can access the file.`"
              />
            </div>

            <div class="col mb-20">
              <RadioGroup 
                v-model="props.row.value.readOnly" 
                :mode="mode" 
                :options="options" 
                label="Read Only"
                placeholder="Read Only"
                name="matchPathsReadOnly"
                v-clean-tooltip="`If enabled, the file can only be read, not modified.`"
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
                      v-clean-tooltip="`Path of the executable that accesses the file.`"
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
        Match Directories
        <i
          v-clean-tooltip="`Match directories for security policies.`"
          class="icon icon-info"
        />
      </h2>
      <ArrayListGrouped 
        v-model="value.spec.file.matchDirectories" 
        :mode="mode" 
        :add-label="'Add Match Directory'"
        :default-add-value="{ dir: '',}">
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
              <RadioGroup 
                v-model="props.row.value.recursive" 
                :mode="mode" 
                :options="options" 
                label="Recursive"
                placeholder="Recursive"
                name="matchDirectoriesRecursive"
                v-clean-tooltip="`If enabled, all subdirectories and files will be recursively matched.`"
              />
            </div>

            <div class="col mb-20">
              <RadioGroup 
                v-model="props.row.value.ownerOnly" 
                :mode="mode" 
                :options="options" 
                label="Owner Only"
                placeholder="Owner Only"
                name="matchDirectoriesOwnerOnly"
                v-clean-tooltip="`If enabled, only the directory owner can access it.`"
              />
            </div>

            <div class="col mb-20">
              <RadioGroup 
                v-model="props.row.value.readOnly" 
                :mode="mode" 
                :options="options" 
                label="Read Only"
                placeholder="Read Only"
                name="matchDirectoriesReadOnly"
                v-clean-tooltip="`If enabled, the directory can only be read, not modified.`"
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
                      v-clean-tooltip="`Path of the executable that accesses the directory.`"
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
        Match Patterns
        <i
          v-clean-tooltip="`Match file patterns using regex.`"
          class="icon icon-info"
        />
      </h2>
      <ArrayListGrouped 
        v-model="value.spec.file.matchPatterns" 
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
                v-clean-tooltip="`A regular expression pattern to match the file.`"
              />
            </div>

            <div class="col mb-20">
              <RadioGroup 
                v-model="props.row.value.ownerOnly" 
                :mode="mode" 
                :options="options" 
                label="Owner Only"
                placeholder="Owner Only"
                name="matchPatternsOwnerOnly"
                v-clean-tooltip="`If enabled, only the owner can access files that match this pattern.`"
              />
            </div>

            <div class="col mb-20">
              <RadioGroup 
                v-model="props.row.value.readOnly" 
                :mode="mode" 
                :options="options" 
                label="Read Only"
                placeholder="Read Only"
                name="matchPatternsReadOnly"
                v-clean-tooltip="`If enabled, the matched files can only be read, not modified.`"
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
      
      if (!this.value.spec.file) {
        this.$set(this.value.spec, 'file', {
          matchPaths: [],
          matchDirectories: [],
          matchPatterns: []
        });
      }
    }
  };
  </script>
  