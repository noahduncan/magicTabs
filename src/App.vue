<template>
  <ResizePanes @widthChange="setVertical">
    <template slot="left">
      <tabs :class="{vertical: leftVertical}" ref="leftPane">
        <tab name="Overview">Overview</tab>
        <tab name="Variables">varaibles</tab>
        <tab name="Tools">tools</tab>
      </tabs>
    </template>
    <template slot="right">
      <tabs :class="{vertical: rightVertical}" ref="rightPane">
        <tab name="Tracking">tracking</tab>
        <tab name="Document Generation">docgen</tab>
        <tab name="Document Processing">docproc</tab>
        <tab name="Filing Portal">filing portal</tab>
        <tab name="Entity Log">entitylog</tab>
      </tabs>
    </template>
  </ResizePanes>
</template>

<script>
    /* eslint-disable */
import ResizePanes from './components/ResizePanes.vue'
import {Tabs,Tab} from 'vue-tabs-component';

export default {
  name: 'app',
  components: {
    ResizePanes,
    Tabs,
    Tab,
  },
  data: function() {
    return {
      leftVertical: false,
      rightVertical: false,
    }
  },
  computed: {
      initialLeftPaneWidth: function () {
          let el = this.$refs.leftPane.$el.getElementsByClassName('tabs-component-tabs')[0];
          el.style.left = -el.clientWidth + 'px';
          return el.clientWidth;
      },
      initialRightPaneWidth: function () {
          let el = this.$refs.rightPane.$el.getElementsByClassName('tabs-component-tabs')[0];
          el.style.left = -el.clientWidth + 'px';
          return el.clientWidth;
      },
  },
  methods: {
      setVertical: function() {
          this.leftVertical = this.initialLeftPaneWidth > this.$refs.leftPane.$el.clientWidth;
          this.rightVertical = this.initialRightPaneWidth > this.$refs.rightPane.$el.clientWidth;
      }
  }
}
</script>

<style lang="scss">
  body, html {
    margin: 0;
    padding: 0;
  }
  .tabs-component {
    height: 100%;
    .tabs-component-panels {
      border: 1px solid black;
      height: 100%;
    }
    .tabs-component-tabs {
      list-style: none;
      clear: both;
      display: inline-flex;
      flex-flow: row nowrap;
      margin: 0;
      padding-left: 10px;

      .tabs-component-tab {
        border: 1px solid black;
        position: relative;
        top: 1px;
        padding: 2px 5px;
        margin-right: 3px;
        border-radius: 5px 5px 0 0;
        font-size: 12px;
        background-color: lightgray;

        &.is-active {
          background-color: white;
          border-bottom-color: white;
        }
        a {
          text-decoration: none;
          color: inherit;
        }
      }
    }
    &.vertical {
      .tabs-component-panels {
          margin-left: 21px;
      }
      .tabs-component-tabs {
        float: left;
        transform: rotate(-90deg);
        transform-origin: right top;
        flex-direction: row-reverse;
        position: absolute;
      }
    }
  }
</style>
