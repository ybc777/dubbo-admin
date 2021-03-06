<!--
  - Licensed to the Apache Software Foundation (ASF) under one or more
  - contributor license agreements.  See the NOTICE file distributed with
  - this work for additional information regarding copyright ownership.
  - The ASF licenses this file to You under the Apache License, Version 2.0
  - (the "License"); you may not use this file except in compliance with
  - the License.  You may obtain a copy of the License at
  -
  -   http://www.apache.org/licenses/LICENSE-2.0
  -
  - Unless required by applicable law or agreed to in writing, software
  - distributed under the License is distributed on an "AS IS" BASIS,
  - WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  - See the License for the specific language governing permissions and
  - limitations under the License.
  -->
<template>
  <div>
    <span style="color:red" v-if="formItemInfo.get('required')">*</span>
    <v-text-field
      v-if="formItemInfo.get('htmlType')==='TEXT'"
      :id="buildItemId()"
      :name="buildItemId()"
      :ref="buildItemId()"
      :label="formItemInfo.get('docName')"
      :placeholder="formItemInfo.get('example')"
      :value="buildDefaultValue()"
      :required="formItemInfo.get('required')"
      :rules="[requiredCheck]"
      @change="itemChange($event)"
      outline
    ></v-text-field>
    <v-text-field
      v-else-if="formItemInfo.get('htmlType')==='TEXT_BYTE'"
      :id="buildItemId()"
      :name="buildItemId()"
      :ref="buildItemId()"
      :label="formItemInfo.get('docName')"
      :placeholder="formItemInfo.get('example')"
      :value="buildDefaultValue()"
      :required="formItemInfo.get('required')"
      :rules="[requiredCheck]"
      @change="itemChange($event)"
      outline
    ></v-text-field>
    <v-text-field
      v-else-if="formItemInfo.get('htmlType')==='TEXT_CHAR'"
      :id="buildItemId()"
      :name="buildItemId()"
      :ref="buildItemId()"
      :label="formItemInfo.get('docName')"
      :placeholder="formItemInfo.get('example')"
      :value="buildDefaultValue()"
      :required="formItemInfo.get('required')"
      :rules="[requiredCheck]"
      @change="itemChange($event)"
      outline
    ></v-text-field>
    <v-text-field
      v-else-if="formItemInfo.get('htmlType')==='NUMBER_INTEGER'"
      :id="buildItemId()"
      :name="buildItemId()"
      :ref="buildItemId()"
      :label="formItemInfo.get('docName')"
      :placeholder="formItemInfo.get('example')"
      :value="buildDefaultValue()"
      :required="formItemInfo.get('required')"
      :rules="[requiredCheck]"
      @change="itemChange($event)"
      outline
    ></v-text-field>
    <v-text-field
      v-else-if="formItemInfo.get('htmlType')==='NUMBER_DECIMAL'"
      :id="buildItemId()"
      :name="buildItemId()"
      :ref="buildItemId()"
      :label="formItemInfo.get('docName')"
      :placeholder="formItemInfo.get('example')"
      :value="buildDefaultValue()"
      :required="formItemInfo.get('required')"
      :rules="[requiredCheck]"
      @change="itemChange($event)"
      outline
    ></v-text-field>
    <v-select
      v-else-if="formItemInfo.get('htmlType')==='SELECT'"
      :id="buildItemId()"
      :name="buildItemId()"
      :ref="buildItemId()"
      :label="formItemInfo.get('docName')"
      :items="buildSelectItem()"
      item-text="label"
      item-value="value"
      :value="buildSelectDefaultValue()"
      :required="formItemInfo.get('required')"
      :rules="[requiredCheck]"
      @change="itemChange($event)"
      outline
    ></v-select>
    <json-editor2
      v-else-if="formItemInfo.get('htmlType')==='TEXT_AREA'"
      :id="buildItemId()"
      :name="buildItemId()"
      :ref="buildItemId()"
      :label="formItemInfo.get('docName')"
      :json="buildJsonDefaultValue()"
      :required="formItemInfo.get('required')"
      :rules="[requiredCheck]"
      :onChange="itemChange"
      :options="{modes: ['code','tree']}"
      style="height:300px;"
      outline
    ></json-editor2>
    <span v-else>{{$t('apiDocsRes.apiForm.unsupportedHtmlTypeTip')}}</span>
  </div>
</template>

<script>
import JsonEditor2 from '@/components/public/JsonEditor2'

export default {
  name: 'ApiFormItem',
  components: {
    JsonEditor2
  },
  props: {
    formItemInfo: {
      type: Map,
      default: function () {
        return new Map()
      }
    },
    formValues: {
      type: Map,
      default: function () {
        return new Map()
      }
    }
  },
  data: () => {
    return {
    }
  },
  watch: {
  },
  methods: {
    buildItemId () {
      return this.formItemInfo.get('paramType') + '@@' +
      this.formItemInfo.get('paramIndex') + '@@' +
      this.formItemInfo.get('javaType') + '@@' +
      this.formItemInfo.get('name') + '@@' +
      this.formItemInfo.get('htmlType')
    },
    requiredCheck (value) {
      if (this.formItemInfo.get('required')) {
        return !!value || this.$t('apiDocsRes.apiForm.requireItemTip')
      } else {
        return true
      }
    },
    buildSelectItem () {
      var allowableValues = this.formItemInfo.get('allowableValues')
      const dataSource = new Array()
      for (var i = 0; i < allowableValues.length; i++) {
        var valueItem = allowableValues[i]
        var dsItem = {}
        dsItem.label = valueItem
        dsItem.value = valueItem
        dataSource.push(dsItem)
      }
      return dataSource
    },
    buildDefaultValue () {
      var defaultValue = this.formItemInfo.get('defaultValue')
      this.formValues.set(this.buildItemId(), defaultValue)
      return defaultValue
    },
    buildSelectDefaultValue () {
      var allowableValues = this.formItemInfo.get('allowableValues')
      this.formValues.set(this.buildItemId(), allowableValues[0])
      return allowableValues[0]
    },
    buildJsonDefaultValue () {
      var defaultValue = JSON.parse(this.formItemInfo.get('subParamsJson'))
      this.formValues.set(this.buildItemId(), defaultValue)
      return defaultValue
    },
    itemChange (e) {
      this.formValues.set(this.buildItemId(), e)
    }
  },
  mounted () {
    // var _refs = this.$refs
    // for (var key in _refs) {
    //   _refs[key].$emit('change')
    // }
    // this.formValues.set(this.buildItemId(), this.formItemInfo.get('defaultValue'))
  }
}
</script>

<style scoped>
</style>
