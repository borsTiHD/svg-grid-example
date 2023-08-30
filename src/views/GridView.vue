<template>
  <div class="program-flow">
    <svg ref="svgContainer" class="svg-container">
      <defs>
        <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="5" refY="3.5" orient="auto">
          <polygon points="0 0, 10 3.5, 0 7" fill="#dfdfdf"/>
        </marker>
      </defs>
      <template v-for="(box, index) in boxes" :key="index">
        <rect
          :x="box.x"
          :y="box.y"
          width="100"
          height="60"
          rx="10"
          ry="10"
          class="box cursor-pointer"
          @click="boxClicked(index)"
        />
        <text :x="box.x + 50" :y="box.y + 35" class="box-text cursor-pointer" @click="boxClicked(index)">{{ box.label }}</text>
        <line
          v-if="index < boxes.length - 1"
          :x1="box.x + 50"
          :y1="box.y + 61"
          :x2="box.x + 50"
          :y2="boxes[index + 1].y - 10"
          class="arrow-line"
          marker-end="url(#arrowhead)"
        />
      </template>
    </svg>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface BoxItem {
  x: number;
  y: number;
  label: string;
}

const boxes = ref<BoxItem[]>([])

// Erstellt zufällige Boxen
const generateBoxes = (count: number) => {
  for (let i = 1; i <= count; i++) {
    boxes.value.push({
      x: 50, // Einfache Berechnung für die X-Position
      y: i * 150, // Y-Position bleibt gleich
      label: `Step ${i}`,
    });
  }
}

// Fügt die Boxen hinzu
generateBoxes(5)

// Klick-Handler für die Boxen
const boxClicked = (index: number) => {
  console.log(`Box ${index + 1} clicked`)
}
</script>

<style>
.program-flow {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  position: relative; /* Position für das Pseudo-Element festlegen */
}

/* Hintergrundraster hinzufügen */
.program-flow::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: linear-gradient(rgba(255, 255, 255, 0.3) 1px, transparent 1px),
                    linear-gradient(90deg, rgba(255, 255, 255, 0.3) 1px, transparent 1px);
  background-size: 20px 20px;
  pointer-events: none; /* Klicks und Interaktionen für das Pseudo-Element deaktivieren */
  z-index: -1; /* Das Pseudo-Element hinter die anderen Inhalte legen */
}


.svg-container {
  width: 100%;
  height: 100%;
}

.box {
  fill: #e0e0e0;
  stroke: #333;
  stroke-width: 2;
  display: flex;
  justify-content: center; /* Horizontal zentrieren */
  align-items: center; /* Vertikal zentrieren */
}

.box-text {
  font-size: 14px;
  font-weight: bold;
  text-anchor: middle;
  font-variant-caps: central;
  text-align: center; /* Text horizontal zentrieren */
  line-height: 1.2; /* Vertikal zentrieren */
}

.arrow-line {
  stroke: #dfdfdf;
  stroke-width: 2;
  marker-end: url(#arrowhead);
}
</style>
