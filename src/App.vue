<!-- eslint-disable vue/no-parsing-error -->
<template>
  <div class="w-screen h-screen flex justify-center items-center">
    <!-- dropdown -->
    <div class="flex-col rounded-md">
      <Select v-model="selectedPiece" />
      <div v-for="y in 8" :key="y" class="flex">
        <div
          v-for="x in 8"
          :key="x"
          class="border border-solid border-black p-4 flex items-center justify-center"
          v-bind:style="
            selectedCell.x === x && selectedCell.y === y
              ? 'background-color:lightblue'
              : validMoves.some((move: any) => move.x === x && move.y === y)
                ? 'background-color:skyblue'
                : ''
          "
          @click="handleCellClick(x, y)"
        >
          <!-- x:{{ x }},y:{{ y }} -->
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { watch } from 'vue'
import { ref } from 'vue'
import Select from './components/Select.vue'

interface coordinates {
  x: number
  y: number
}

const selectedPiece = ref<string>('rook')

const validMoves = ref<coordinates[]>([])

const selectedCell = ref<coordinates>({ x: 0, y: 0 })

const handleCellClick = (x: number, y: number) => {
  selectedCell.value = { x, y }

  const kingMoves = [
    { x, y: y + 1 },
    { x, y: y - 1 },
    { x: x - 1, y },
    { x: x + 1, y },
    { x: x + 1, y: y + 1 },
    { x: x - 1, y: y - 1 },
    { x: x - 1, y: y + 1 },
    { x: x + 1, y: y - 1 }
  ]

  const rookMoves = [
    { x, y: y - 1 },
    { x, y: y - 2 },
    { x, y: y - 3 },
    { x, y: y - 4 },
    { x, y: y - 5 },
    { x, y: y - 6 },
    { x, y: y - 7 },

    { x, y: y + 1 },
    { x, y: y + 2 },
    { x, y: y + 3 },
    { x, y: y + 4 },
    { x, y: y + 5 },
    { x, y: y + 6 },
    { x, y: y + 7 },

    { y, x: x + 1 },
    { y, x: x + 2 },
    { y, x: x + 3 },
    { y, x: x + 4 },
    { y, x: x + 5 },
    { y, x: x + 6 },
    { y, x: x + 7 },

    { y, x: x - 1 },
    { y, x: x - 2 },
    { y, x: x - 3 },
    { y, x: x - 4 },
    { y, x: x - 5 },
    { y, x: x - 6 },
    { y, x: x - 7 }
  ]

  const horseMoves = [
    { y, x: x - 1 },
    { y, x: x - 2 },
    { y: y - 1, x: x - 2 },
    { y: y + 1, x: x - 2 },

    { y, x: x + 1 },
    { y, x: x + 2 },
    { y: y - 1, x: x + 2 },
    { y: y + 1, x: x + 2 },

    { x, y: y - 1 },
    { x, y: y - 2 },
    { x: x + 1, y: y - 2 },
    { x: x - 1, y: y - 2 },

    { x, y: y + 1 },
    { x, y: y + 2 },
    { x: x - 1, y: y + 2 },
    { x: x + 1, y: y + 2 }
  ]

  const queenMoves = [
    { x, y: y - 1 },
    { x, y: y - 2 },
    { x, y: y - 3 },
    { x, y: y - 4 },
    { x, y: y - 5 },
    { x, y: y - 6 },
    { x, y: y - 7 },

    { x, y: y + 1 },
    { x, y: y + 2 },
    { x, y: y + 3 },
    { x, y: y + 4 },
    { x, y: y + 5 },
    { x, y: y + 6 },
    { x, y: y + 7 },

    { y, x: x + 1 },
    { y, x: x + 2 },
    { y, x: x + 3 },
    { y, x: x + 4 },
    { y, x: x + 5 },
    { y, x: x + 6 },
    { y, x: x + 7 },

    { y, x: x - 1 },
    { y, x: x - 2 },
    { y, x: x - 3 },
    { y, x: x - 4 },
    { y, x: x - 5 },
    { y, x: x - 6 },
    { y, x: x - 7 },

    { x: x + 1, y: y + 1 },
    { x: x + 2, y: y + 2 },
    { x: x + 3, y: y + 3 },
    { x: x + 4, y: y + 4 },
    { x: x + 5, y: y + 5 },
    { x: x + 6, y: y + 6 },
    { x: x + 7, y: y + 7 },

    { x: x - 1, y: y - 1 },
    { x: x - 2, y: y - 2 },
    { x: x - 3, y: y - 3 },
    { x: x - 4, y: y - 4 },
    { x: x - 5, y: y - 5 },
    { x: x - 6, y: y - 6 },
    { x: x - 7, y: y - 7 },

    { x: x - 1, y: y + 1 },
    { x: x - 2, y: y + 2 },
    { x: x - 3, y: y + 3 },
    { x: x - 4, y: y + 4 },
    { x: x - 5, y: y + 5 },
    { x: x - 6, y: y + 6 },
    { x: x - 7, y: y + 7 },

    { x: x + 1, y: y - 1 },
    { x: x + 2, y: y - 2 },
    { x: x + 3, y: y - 3 },
    { x: x + 4, y: y - 4 },
    { x: x + 5, y: y - 5 },
    { x: x + 6, y: y - 6 },
    { x: x + 7, y: y - 7 }
  ]

  const bishopMoves = [
    { x: x + 1, y: y + 1 },
    { x: x + 2, y: y + 2 },
    { x: x + 3, y: y + 3 },
    { x: x + 4, y: y + 4 },
    { x: x + 5, y: y + 5 },
    { x: x + 6, y: y + 6 },
    { x: x + 7, y: y + 7 },

    { x: x - 1, y: y - 1 },
    { x: x - 2, y: y - 2 },
    { x: x - 3, y: y - 3 },
    { x: x - 4, y: y - 4 },
    { x: x - 5, y: y - 5 },
    { x: x - 6, y: y - 6 },
    { x: x - 7, y: y - 7 },

    { x: x - 1, y: y + 1 },
    { x: x - 2, y: y + 2 },
    { x: x - 3, y: y + 3 },
    { x: x - 4, y: y + 4 },
    { x: x - 5, y: y + 5 },
    { x: x - 6, y: y + 6 },
    { x: x - 7, y: y + 7 },

    { x: x + 1, y: y - 1 },
    { x: x + 2, y: y - 2 },
    { x: x + 3, y: y - 3 },
    { x: x + 4, y: y - 4 },
    { x: x + 5, y: y - 5 },
    { x: x + 6, y: y - 6 },
    { x: x + 7, y: y - 7 }
  ]

  const pawnMoves = [{ x, y: y - 1 }]

  switch (selectedPiece.value) {
    case 'pawn':
      validMoves.value = pawnMoves
      break
    case 'king':
      validMoves.value = kingMoves
      break
    case 'rook':
      validMoves.value = rookMoves
      break
    case 'queen':
      validMoves.value = queenMoves
      break
    case 'horse':
      validMoves.value = horseMoves
      break
    case 'bishop':
      validMoves.value = bishopMoves
      break
    default:
      validMoves.value = []
      break
  }
}

watch(selectedPiece, () => {
  validMoves.value = []
  selectedCell.value = { x: 0, y: 0 }
})
</script>
