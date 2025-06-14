# Scoped Slot

For more information on ScopedSlot, see the [official Vue documentation](https://vuejs.org/v2/guide/components-slots.html#Scoped-Slots).

:::warning
Both slot and scoped slots are supported, but the use of the v-slot directive is recommended.
:::

## pvtAttr

If you want to customize the label of each pivot attribute, you can use the `pvtAttr` slot.

```vue{10-15}
<template>
  <VuePivottableUi
    :data="[
      { color: 'blue', shape: 'circle' },
      { color: 'red', shape: 'triangle' },
    ]"
    :rows="['color']"
    :cols="['shape']"
  >
    <template #pvtAttr="{ attrName }">
      <i class="fas fa-bars"></i>
      <span style="margin-left: 5px">
        {{ capitalize(attrName) }}
      </span>
    </template>
  </VuePivottableUi>
</template>

<script setup>
import { VuePivottableUi } from "vue-pivottable";
import "vue-pivottable/dist/vue-pivottable.css";

function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}
</script>
```

<iframe
  src="https://stackblitz.com/edit/vitejs-vite-uvgnlrhv?embed=1&file=src%2FApp.vue&hideExplorer=1&hideNavigation=1&view=preview"
  width="100%"
  height="500"
  style="border:0; border-radius: 4px; overflow:hidden;"
  title="Vite Vue3 Sample"
  allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
  sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts allow-popups-to-escape-sandbox"
></iframe>

## output

```vue{11-36}
<template>
  <div>
    <vue-pivottable-ui
      :data="[
        { color: 'blue', shape: 'circle' },
        { color: 'red', shape: 'triangle' },
      ]"
      :rows="['color']"
      :cols="['shape']"
    >
      <template v-if="!loaded" v-slot:output="{ pivotData }">
        <div v-if="!viewTable">
          <div class="btn-group">
            <a class="btn btn-sm btn-primary" @click="showTable">
              View Table
            </a>
            <a class="btn btn-sm btn-warning" @click="otherAction(pivotData)"
              >Other action
            </a>
          </div>
        </div>
        <template v-else>
          <table-renderer
            v-if="pivotData.props.rendererName === 'Table'"
            :data="pivotData.props.data"
            :props="pivotData.props"
          >
          </table-renderer>
          <heatmap-renderer
            v-if="pivotData.props.rendererName === 'Table Heatmap'"
            :data="pivotData.props.data"
            :props="pivotData.props"
          >
          </heatmap-renderer>
        </template>
      </template>
    </vue-pivottable-ui>
    <button
      class="btn btn-sm btn-secondary mt-1"
      :disabled="!loaded"
      @click="reset"
    >
      <i class="fas fa-redo mr-25"></i>
      redo
    </button>
  </div>
</template>

<script>
import { VuePivottableUi, Renderer } from "vue-pivottable";
import "vue-pivottable/dist/vue-pivottable.css";
const HeatmapRenderer = Renderer.TableRenderer["Table Heatmap"];
const TableRenderer = Renderer.TableRenderer["Table"];
export default {
  components: {
    VuePivottableUi,
    HeatmapRenderer,
    TableRenderer,
  },
  data() {
    return {
      viewTable: false,
      loaded: false,
    };
  },
  methods: {
    showTable() {
      this.viewTable = !this.viewTable;
      this.loaded = true;
    },
    otherAction(pivotData) {
      alert(`All Total Count: ${pivotData.allTotal.count}`);
    },
    reset() {
      this.viewTable = false;
      this.loaded = false;
    },
  },
};
</script>
```

<iframe src="https://codesandbox.io/embed/vue-pivottable-ui-outputscopedslot-rcp9k?fontsize=14&hidenavigation=1&theme=light&view=preview"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="vue-pivottable-ui_OutputScopedSlot"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>
