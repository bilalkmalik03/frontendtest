<template>
  <div class="min-h-screen bg-gradient-to-br from-slate-900 via-slate-800 to-slate-900 p-4 md:p-8">
    <div class="max-w-7xl mx-auto">
      <h1 class="text-3xl md:text-4xl font-bold text-white mb-6 text-center">
        Chess.com Board Click Tracker
      </h1>
      
      <div class="flex flex-col lg:flex-row gap-6">
        <!-- Chessboard Container -->
        <div class="flex-1 flex items-center justify-center">
          <div class="w-full max-w-2xl aspect-square">
            <div class="grid grid-cols-8 gap-0 w-full h-full border-4 border-amber-900 rounded-lg overflow-hidden shadow-2xl">
              <button
                v-for="(square, index) in allSquares"
                :key="square"
                @click="handleSquareClick(square)"
                :class="[
                  'relative aspect-square flex items-center justify-center transition-all duration-200 hover:opacity-80',
                  isLightSquare(index) ? 'bg-amber-100' : 'bg-amber-800',
                  highlightedSquare === square ? 'ring-4 ring-yellow-400 ring-inset z-10' : ''
                ]"
              >
                <!-- File labels (bottom row) -->
                <span
                  v-if="isBottomRow(index)"
                  :class="[
                    'absolute bottom-0.5 right-0.5 text-xs font-semibold',
                    isLightSquare(index) ? 'text-amber-800' : 'text-amber-100'
                  ]"
                >
                  {{ getFile(index) }}
                </span>
                
                <!-- Rank labels (left column) -->
                <span
                  v-if="isLeftColumn(index)"
                  :class="[
                    'absolute top-0.5 left-0.5 text-xs font-semibold',
                    isLightSquare(index) ? 'text-amber-800' : 'text-amber-100'
                  ]"
                >
                  {{ getRank(index) }}
                </span>
                
                <!-- Highlight indicator -->
                <div
                  v-if="highlightedSquare === square"
                  class="absolute inset-0 bg-yellow-400 opacity-40"
                />
              </button>
            </div>
          </div>
        </div>

        <!-- Sidebar -->
        <div class="lg:w-80 w-full">
          <div class="bg-slate-800 rounded-lg shadow-xl border border-slate-700 overflow-hidden h-full max-h-[600px] flex flex-col">
            <div class="bg-gradient-to-r from-amber-600 to-amber-700 p-4 flex justify-between items-center">
              <h2 class="text-xl font-bold text-white">Click History</h2>
              <button
                @click="clearHistory"
                class="px-3 py-1 bg-white text-amber-700 rounded-md text-sm font-semibold hover:bg-amber-50 transition-colors"
              >
                Clear
              </button>
            </div>
            
            <div class="p-4 flex-1 overflow-y-auto">
              <div v-if="clickedSquares.length === 0" class="text-center text-slate-400 py-8">
                <p class="text-sm">Click on squares to track them</p>
              </div>
              
              <div v-else class="space-y-2">
                <div
                  v-for="(click, index) in clickedSquares"
                  :key="index"
                  class="bg-slate-700 rounded-lg p-3 border border-slate-600 hover:border-amber-500 transition-colors"
                >
                  <div class="flex items-center justify-between">
                    <div class="flex items-center gap-3">
                      <span class="flex items-center justify-center w-8 h-8 bg-amber-600 text-white rounded-full font-bold text-sm">
                        {{ click.order }}
                      </span>
                      <span class="text-2xl font-bold text-white">
                        {{ click.square }}
                      </span>
                    </div>
                    <span class="text-xs text-slate-400">
                      {{ click.timestamp }}
                    </span>
                  </div>
                </div>
              </div>
            </div>
            
            <div class="bg-slate-900 p-3 border-t border-slate-700">
              <div class="text-sm text-slate-300 text-center">
                Total clicks: <span class="font-bold text-amber-400">{{ clickedSquares.length }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ChessBoard',
  data() {
    return {
      clickedSquares: [],
      highlightedSquare: null,
      files: ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'],
      ranks: ['8', '7', '6', '5', '4', '3', '2', '1']
    }
  },
  computed: {
    allSquares() {
      const squares = []
      this.ranks.forEach(rank => {
        this.files.forEach(file => {
          squares.push(`${file}${rank}`)
        })
      })
      return squares
    }
  },
  methods: {
    handleSquareClick(square) {
      this.highlightedSquare = square
      this.clickedSquares.push({
        square,
        timestamp: new Date().toLocaleTimeString(),
        order: this.clickedSquares.length + 1
      })
    },
    clearHistory() {
      this.clickedSquares = []
      this.highlightedSquare = null
    },
    isLightSquare(index) {
      const row = Math.floor(index / 8)
      const col = index % 8
      return (row + col) % 2 === 0
    },
    isBottomRow(index) {
      return Math.floor(index / 8) === 7
    },
    isLeftColumn(index) {
      return index % 8 === 0
    },
    getFile(index) {
      return this.files[index % 8]
    },
    getRank(index) {
      return this.ranks[Math.floor(index / 8)]
    }
  }
}
</script>