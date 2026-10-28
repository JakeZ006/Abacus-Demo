<script setup>
import { computed, reactive, ref } from 'vue'

let rowCount = reactive([0, 0, 0, 0, 0, 0, 0, 0, 0, 0])

let count = computed(() => {
  let newCount = 0
  rowCount.forEach((item, index, arr) => {
    for (let i = index - 1; i >= 0; i--) {
      item *= 10
    }
    newCount += item
  })
  return newCount
})

function up(row) {
  let currentBead = document.getElementById(`row-${row}-bead-${rowCount[row - 1] + 1}`)
  let nextBead = document.getElementById(`row-${row}-bead-${rowCount[row - 1] + 2}`)

  rowCount[row - 1]++

  if (rowCount[row - 1] < 10) {
    currentBead.style.marginLeft = '0px'
    nextBead.style.marginLeft = '255px'
  } else if ((rowCount[row - 1] = 10)) {
    currentBead.style.marginRight = '255px'
    currentBead.style.marginLeft = '0px'
    setTimeout(() => {
      document.getElementById(`row-${row + 1}-bead-${rowCount[row] + 1}`).style.marginLeft = '0px'
      document.getElementById(`row-${row + 1}-bead-${rowCount[row] + 2}`).style.marginLeft =
        '255px'
    }, 200)
    setTimeout(() => {
      currentBead.style.marginRight = '0px'
      rowCount[row - 1] = 0
      rowCount[row]++
    }, 500)
  }
}

function down(row) {
  let currentBead = document.getElementById(`row-${row}-bead-${rowCount[row - 1] }`)
  let nextBead = document.getElementById(`row-${row}-bead-${rowCount[row - 1] + 1}`)

  rowCount[row - 1]--

  if (rowCount[row - 1] < 10) {
    currentBead.style.marginLeft = '255px'
    nextBead.style.marginLeft = '0px'
  } else if ((rowCount[row - 1] = 0)) {
    currentBead.style.marginRight = '255px'
    currentBead.style.marginLeft = '0px'
    setTimeout(() => {
      document.getElementById(`row-${row + 1}-bead-${rowCount[row] + 1}`).style.marginLeft = '0px'
      document.getElementById(`row-${row + 1}-bead-${rowCount[row] + 2}`).style.marginLeft =
        '255px'
    }, 200)
    setTimeout(() => {
      currentBead.style.marginRight = '0px'
      rowCount[row - 1] = 0
      rowCount[row]++
    }, 500)
  }
}
</script>

<template>
  <div class="abacus-body">
    <h1 class="count">{{ count.toLocaleString() }}</h1>
    <div v-for="row in 10" :key="`row-${row}`" class="row" :id="`row-${row}`">

      <button :id="`up-button${row}`" @click="up(row)"><</button>
      <div class="row-rod">
        <div
          v-for="bead in 10"
          :key="`row-${row}-bead-${bead}`"
          :id="`row-${row}-bead-${bead}`"
          class="bead"
          :style="`background-color: hsl(${row * 18 - 20}, 70%, 55%);`"
        ></div>
      </div>
      <button :id="`down-button${row}`" @click="rowCount[row - 1] > 0 ? down(row) : {}">></button>
      <p class="digit-display">{{"1" + ("0".repeat(row - 1))}}</p>
    </div>
  </div>
</template>

<style lang="scss">
.digit-display{
  margin-left: 32px;
  font: bold 18px sans-serif;
  align-self: center;
}
.abacus-body {
  margin: 200px auto;
  display: flex;
  flex-direction: column-reverse;
  gap: 24px;
  border: 2px solid black;
  border-radius: 16px;
  padding: 16px;
  width: fit-content;
}

.row {
  display: flex;
  button {
    width: 50px;
  }
}
.count {
  text-align: center;
  font-size: 50px;
}
.row-rod {
  display: flex;
  flex-direction: column;
  align-content: end;
  flex-wrap: wrap;
  width: 700px;
  height: 50px;
  gap: 5px;
  background-image: linear-gradient(to bottom, transparent 36%, grey 40% , grey 60%, transparent 64% );
}

.bead {
  margin-right: 0;
  width: 40px;
  height: 50px;
  border-radius: 20px;
  transition: all ease 200ms;
}
</style>
