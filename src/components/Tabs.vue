<template>
  <div>
    <ul class="tabs" :class="{[classPrefix+'-labs']:classPrefix}">
      <li v-for="item in dataSource" :key="item.value"
          class="tabs-item" :class="liClass(item)" @click="select(item)"
      >{{item.text}}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import {Component, Prop} from 'vue-property-decorator';

type DataSourceItem = { text: string, value: string }
@Component
export default class Tabs extends Vue {
  @Prop({required: true, type: Array})
  dataSource!: DataSourceItem[];
  @Prop(String)
  readonly value!: string;
  @Prop(String)
  classPrefix?: string;

  liClass(item: DataSourceItem) {
    return {
      [this.classPrefix + '-tabs-item']: this.classPrefix,
      selected: item.value === this.value
    };
  }

  select(item: DataSourceItem) {
    this.$emit('update:value', item.value);
  }
}
</script>

<style lang="scss" scoped>
  .tabs {
    background: #c4c4c4;
    font-size: 24px;
    display: flex;
    text-align: center;

    &-item {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 50%;
      height: 64px;
      position: relative;

      &.selected::after {
        content: '';
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 4px;
        background: #333333;
      }
    }
  }
</style>