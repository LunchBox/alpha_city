<script setup>
import { ref, computed } from 'vue'

const width = ref(512)
const height = ref(384)

const unit = ref(16)

const units = [8, 16, 32, 64, 128]

const cStyle = computed(() => {
  return {
    width: width.value + 'px',
    height: height.value + 'px'
  }
})

const cells = computed(() => width.value / unit.value * height.value / unit.value)

const cellStyle = computed(() => {
  return {
    width: unit.value + 'px',
    height: unit.value + 'px'
  }  
})

const areas = ref([])
const currentArea = ref(null)

function addArea() {
  const area = { name: "Unnamed Area", cells: new Set() }
  areas.value.push(area)
  currentArea.value = area
}

function toggleMark(mark) {
  console.log(mark)
  if (!currentArea.value) return 
 
  if (currentArea.value.cells.has(mark)) {
    currentArea.value.cells.delete(mark)
  } else {
    currentArea.value.cells.add(mark)
  }
}

function hasMark(mark) {
  if (!currentArea.value) return 

  return currentArea.value.cells.has(mark)
}

function mark(idx) {
  return `cell-${unit.value}-${idx}`
}

</script>

<template>
  <div>
    <h1>Map Editor</h1>

    <div>
      Unit: 
      <a href="#" v-for="u in units" :key="u" @click.prevent="unit = u">{{u}}px</a>
    </div>

    <div class='canvas' :style="cStyle">
      <div class='cell' :class="{active: hasMark(mark(idx))}" :data-mark="mark(idx)" v-for="(c, idx) in cells" :key="c" :style="cellStyle" @click="toggleMark(mark(idx))">
      </div>
    </div>

    <div> 
      <h3>Areas</h3>
      <a href="#" @click.prevent="addArea">+ Add Area</a>
      <div>
        <ul>
          <li v-for="area in areas" :key="area" :class="{active: area === currentArea}">
            <a href="#" @click.prevent="currentArea = area">{{ area.name }}</a>
          </li>
        </ul>
      </div>
    </div>
  </div>

</template>

<style scoped>
  .canvas {
    border: 1px solid #ccc;
    box-sizing: content-box;

    display: flex;
    flex-flow: wrap;
  }

  .cell {
    display: block;
    box-sizing: border-box;
    border: 1px solid #efefef;
    margin: 0;
  }

  .cell.active {
    background: lightgreen;
  }


  li.active:after {
    content: " <---";
  }
</style>
