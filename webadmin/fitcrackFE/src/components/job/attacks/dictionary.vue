<!--
   * Author : see AUTHORS
   * Licence: MIT, see LICENSE
-->

<template>
  <div>
    <v-card-title>
      <span>Select dictionary<span class="required primary--text"> *</span></span>
    </v-card-title>
    <dict-selector
      v-model="leftDicts"
      select-all
      @input="checkValid"
    />

    <v-divider />
    <v-card-title>
      <span>Fragmentation of dictionaries</span>
    </v-card-title>
    <v-radio-group
      v-model="distributionMode"
    >
      <v-radio
        label="On server"
        :value="0"
      ></v-radio>
      <v-radio
        label="On hosts"
        :value="1"
      ></v-radio>
    </v-radio-group>
    <v-divider />

    <div
      v-if="distributionMode == 1"
    >
      <v-card-title>
        <span>Deployment of dictionaries</span>
      </v-card-title>
      <v-radio-group
        v-model="dictDeploymentMode"
      >
        <v-radio
          label="Automatically send dictionaries to hosts"
          :value="0"
        ></v-radio>
        <v-radio
          v-if="leftDicts.length == 1"
          label="Use local (prestored) dictionaries on hosts"
          :value="1"
        ></v-radio>
      </v-radio-group>
      <v-divider />
    </div>

    <v-card-title>
      <span>Select rule file</span>
    </v-card-title>
    <rules-selector
      v-model="rules"
      @input="checkValid"
    />

    <v-divider />
    <v-checkbox
      v-if="!$optimizedOnly"
       v-model="optimized"
       label="Use optimized computing kernels (limits password length, disable for passwords over 256 chars long)"
    />
  </div>
</template>

<script>
  import dictSelector from '@/components/selector/dictionarySelector.vue'
  import ruleSelector from '@/components/selector/rulesSelector.vue'

  import {mapTwoWayState} from 'spyfu-vuex-helpers'
  import {twoWayMap} from '@/store'

  export default {
    name: "Dictionary",
    components: {
      'dict-selector': dictSelector,
      'rules-selector': ruleSelector
    },
    computed: mapTwoWayState('jobForm', twoWayMap(['leftDicts', 'rules', 'optimized', 'distributionMode', 'dictDeploymentMode'])),
    methods: {
      checkValid: function () {
        if (this.leftDicts.length > 0) {
          return true
        }
        return false
      }
    }
  }
</script>

<style scoped>
  .required {
    font-weight: bold;
    color: #000;
  }
</style>
