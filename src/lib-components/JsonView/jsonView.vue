<template>

  <div :class="['json-view-container',theme,`deep-${currentDeep}`]">
    <div
      :class="['json-view', length ? 'closeable' : '']"
      :style="{fontSize:fontSize+'px',lineHeight:lineHeight+'px'}"
    >
      <!--icon-style-square-->
      <span v-if="length && iconStyle==='square'" class="angle" @click="toggleClose">
        <svg
          v-if="innerclosed"
          :fill="iconColors[0]"
          width="1em"
          height="1em"
          viewBox="0 0 1792 1792"
          style="vertical-align: middle; color: rgb(42, 161, 152); height: 1em; width: 1em;"
        >
          <path d="M1344 800v64q0 14-9 23t-23 9h-352v352q0 14-9 23t-23 9h-64q-14 0-23-9t-9-23v-352h-352q-14 0-23-9t-9-23v-64q0-14 9-23t23-9h352v-352q0-14 9-23t23-9h64q14 0 23 9t9 23v352h352q14 0 23 9t9 23zm128 448v-832q0-66-47-113t-113-47h-832q-66 0-113 47t-47 113v832q0 66 47 113t113 47h832q66 0 113-47t47-113zm128-832v832q0 119-84.5 203.5t-203.5 84.5h-832q-119 0-203.5-84.5t-84.5-203.5v-832q0-119 84.5-203.5t203.5-84.5h832q119 0 203.5 84.5t84.5 203.5z" />
        </svg>
        <svg
          v-if="!innerclosed"
          :fill="iconColors[1]"
          width="1em"
          height="1em"
          viewBox="0 0 1792 1792"
          style="vertical-align: middle; color: rgb(88, 110, 117); height: 1em; width: 1em;"
        >
          <path d="M1344 800v64q0 14-9 23t-23 9h-832q-14 0-23-9t-9-23v-64q0-14 9-23t23-9h832q14 0 23 9t9 23zm128 448v-832q0-66-47-113t-113-47h-832q-66 0-113 47t-47 113v832q0 66 47 113t113 47h832q66 0 113-47t47-113zm128-832v832q0 119-84.5 203.5t-203.5 84.5h-832q-119 0-203.5-84.5t-84.5-203.5v-832q0-119 84.5-203.5t203.5-84.5h832q119 0 203.5 84.5t84.5 203.5z" />
        </svg>
      </span>
      <!--icon-style-circle-->
      <span v-if="length&& iconStyle==='circle'" class="angle" @click="toggleClose">
        <svg
          v-if="!innerclosed"
          viewBox="0 0 24 24"
          :fill="iconColors[0]"
          preserveAspectRatio="xMidYMid meet"
          style="vertical-align: middle; color: rgb(1, 160, 228); height: 1em; width: 1em;"
        >
          <path d="M12,20C7.59,20 4,16.41 4,12C4,7.59 7.59,4 12,4C16.41,4 20,7.59 20,12C20,16.41 16.41,20 12,20M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2M7,13H17V11H7" />
        </svg>
        <svg
          v-if="innerclosed"
          viewBox="0 0 24 24"
          :fill="iconColors[1]"
          preserveAspectRatio="xMidYMid meet"
          style="vertical-align: middle; color: rgb(161, 106, 148); height: 1em; width: 1em;"
        >
          <path d="M12,20C7.59,20 4,16.41 4,12C4,7.59 7.59,4 12,4C16.41,4 20,7.59 20,12C20,16.41 16.41,20 12,20M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2M13,7H11V11H7V13H11V17H13V13H17V11H13V7Z" />
        </svg>
      </span>
      <!--icon-style-triangle-->
      <span v-if="length&& iconStyle==='triangle'" class="angle" @click="toggleClose">
        <svg
          v-if="!innerclosed"
          viewBox="0 0 15 15"
          :fill="iconColors[0]"
          style="vertical-align: top; color: #3c4047; height: 1em; width: 1em; padding-left: 2px;"
        >
          <path d="M0 5l6 6 6-6z" />
        </svg>
        <svg
          v-if="innerclosed"
          viewBox="0 0 15 15"
          :fill="iconColors[1]"
          style="vertical-align: top; color: #3c4047; height: 1em; width: 1em; padding-left: 2px;"
        >
          <path d="M0 14l6-6-6-6z" />
        </svg>
      </span>
      <div class="content-wrap">
        <p :class="['first-line',length>0?'pointer':'']" @click="toggleClose">

          <template>
            <slot
              :data="{
                jsonKey,
                path:path,
                value:data,
                pathType:pathType
              }"
            >
              <span v-if="jsonKey" class="json-key">"{{ jsonKey }}":</span>
            </slot>
          </template>
          <template>
            <slot
              :data="{
                jsonKey,
                path:path,
                length,
                value:data,
                pathType:pathType,
                prefix,
                innerclosed,
                subfix,
                isArray,
                isLast,
                currentDeep
              }"
              name="prefix"
            >
              <span v-if="length">{{ prefix }}{{ innerclosed ? ('...' + subfix) : '' }}
                <span class="json-note">{{ innerclosed ? (length+' items') : '' }}</span>
              </span>
              <span v-if="!length">{{ `${isArray ? '[]' : '{}'}${isLast?'':','}` }}</span>
            </slot>
          </template>
          <!--  <span v-if="length">{{ prefix }}{{ innerclosed ? ('...' + subfix) : '' }}
            <span class="json-note">{{ innerclosed ? (length+' items') : '' }}</span>
          </span>
          <span v-if="!length">{{ `${isArray ? '[]' : '{}'}${isLast?'':','}` }}</span> -->
        </p>
        <div v-if="!innerclosed && length" class="json-body">
          <template v-for="(item, index) in items">
            <json-view
              v-if="item.isJSON"
              :key="index"
              :closed="isClose()"
              :data="item.value"
              :json-key="item.key"
              :current-deep="templateDeep+1"
              :deep="deep"
              :icon-style="iconStyle"
              :theme="theme"
              :font-size="fontSize"
              :line-height="lineHeight"
              :icon-color="iconColors"
              :is-last="index === items.length - 1"
              :has-siblings="item.hasSiblings"
              :path="item.path"
              :path-type="item.pathType"
            >

              <template #default="{data}">
                <slot :data="data" />
              </template>
              <template #jsonValue="{data}">
                <slot :data="data" name="jsonValue" />
              </template>
              <template #prefix="{data}">
                <slot :data="data" name="prefix" />
              </template>

            </json-view>
            <p v-else :key="index" class="json-item">
              <slot
                :data="{
                  jsonKey:isArray?'':item.key,
                  path:item.path,
                  value:item.value,
                  pathType:item.pathType
                }"
              >
                <span class="json-key">
                  {{ (isArray ? '' : '"' + item.key + '":') }}
                </span>
              </slot>
              <slot
                :data="{
                  jsonKey:isArray?'':item.key,
                  path:item.path,
                  value:item.value,
                  pathType:item.pathType,
                  class:['json-value',getDataType(item.value)]
                }"
                name="jsonValue"
              >
                <span :class="['json-value',getDataType(item.value)]">
                  {{ `${getDataType(item.value)==='string'?'"':''}${formatValue(item.value)}${getDataType(item.value)==='string'?'"':''}${index ===items.length - 1 ? '' : ','}` }}
                </span>
              </slot>

            </p>
          </template>
          <span v-if="!innerclosed" class="base-line" />
        </div>
        <p v-if="!innerclosed " class="last-line">
          <span>{{ subfix }}</span>
        </p>
      </div>
    </div>

  </div>

</template>
<script>
import jsonView from './jsonView'

export default jsonView
</script>
<style scoped lang="less">
    @import "./style/index";
    @import "./style/on-dark";
    @import "./style/vs-code";

</style>
