<script setup>
  import GameGrid from './assets/components/GameGrid.vue';
  import GameInfo from './assets/components/GameInfo.vue';
  import Controls from './assets/components/Controls.vue';
  import { ref } from 'vue';
  const cellBgColor = (cellValue) => {
      switch(cellValue) {
        case '':
          return 'bg-gray-500'
        case 2:
          return 'bg-gray-200';
        case 4:
          return 'bg-orange-100';
        case 8:
          return 'bg-orange-200';
        case 16:
          return 'bg-orange-300';  
        case 32:
          return 'bg-red-300';
        case 64:
          return 'bg-red-500';
        case 128:
          return 'bg-yellow-100';
        case 256:
          return 'bg-yellow-100';   
        case 512:
          return 'bg-yellow-100';
        case 1024:
          return 'bg-yellow-100';
        case 2048:
          return 'bg-yellow-100';
      } 
  }

  const gameGrid = ref([
      [{value: '', color: ''}, {value: '', color: ''}, {value: '', color: ''}, {value: '', color: ''},],
      [{value: '', color: ''}, {value: '', color: ''}, {value: '', color: ''}, {value: '', color: ''},],
      [{value: '', color: ''}, {value: '', color: ''}, {value: '', color: ''}, {value: '', color: ''},],
      [{value: '', color: ''}, {value: '', color: ''}, {value: '', color: ''}, {value: '', color: ''},]
  ].map(x => x.map(t => {
    t.color = function () {return cellBgColor(this.value)};
    console.log(t)
    return t
  })))
  
  let score = ref(0)

  console.log(gameGrid.value)

  const startGame = () => {
    fillRandomCell(gameGrid.value);
    fillRandomCell(gameGrid.value);
  }

  const handleSentDirection = (sentDirection) => {
      swipeTheGrid(sentDirection, gameGrid.value);
  }

  const fillRandomCell = (grid) => {
    
    if (checkWinLossCondition(grid) === 'stop') {
      return
    } else {
      if (checkWinLossCondition(grid)) {
        alert ('you lost')
        return
      }
    }
    const randomRow = Math.floor(Math.random() * 4);
    const randomCell = Math.floor(Math.random() * 4);
    if (grid[randomRow][randomCell].value === '') {
      grid[randomRow][randomCell].value = 2;
    } else {
      fillRandomCell(grid)
    }
  }

  const swipeTheGrid = (direction, grid) => {
    if (direction === 'left' || direction === 'right') {
       for (let i = 0; i < 4; i++) {
        let rowToMutate = grid[i].map(x => x = x.value).filter(x => x !== '')
        if (rowToMutate.length > 1) {
          if (direction === 'right') {
          for (let t = rowToMutate.length - 1; t >= 0; t--) {
            if (t > 0) {
              if (rowToMutate[t] === rowToMutate[t - 1]) {
                rowToMutate[t - 1] *= 2;
                score.value += rowToMutate[t - 1];
                rowToMutate[t] = '';
                t--
              }
            }
          }
        } else {
          for (let t = 0; t < rowToMutate.length - 1; t++) {
            if (t < rowToMutate.length - 1) {
              if (rowToMutate[t] === rowToMutate[t + 1]) {
                rowToMutate[t + 1] *= 2;
                score.value += rowToMutate[t + 1];
                rowToMutate[t] = '';
                t++
              }
            }
          }
        }
        } 
        rowToMutate = rowToMutate.filter(x => x !== '')
        while (rowToMutate.length !== 4) {
          direction === 'left' ?  rowToMutate.push('') : rowToMutate.unshift('');
        }
        console.log(rowToMutate)
        grid[i] = grid[i].map((x, ind)=> {
          x.value = rowToMutate[ind];
          return x
        })
        console.log(grid)
      }
    } else {
      for (let i = 0; i < 4; i++) {
        let rowToMutate = [];
        for (let t = 0; t < 4; t++) {
          if (grid[t][i].value !== ''){
          rowToMutate.push(grid[t][i].value);
          }
        }  
        if (rowToMutate.length > 1) {
          if (direction === 'down') {
          for (let t = rowToMutate.length - 1; t >= 0; t--) {
            if (t > 0) {
              if (rowToMutate[t] === rowToMutate[t - 1]) {
                rowToMutate[t - 1] *= 2;
                score.value += rowToMutate[t - 1];
                rowToMutate[t] = '';
                t--
              }
            }
          } 
        } else {
          for (let t = 0; t < rowToMutate.length - 1; t++) {
            if (t < rowToMutate.length - 1) {
              if (rowToMutate[t] === rowToMutate[t + 1]) {
                rowToMutate[t + 1] *= 2;
                score.value += rowToMutate[t + 1];
                rowToMutate[t] = '';
                t++
              }
            }
          }
        }
        }
        console.log(rowToMutate)
        rowToMutate = rowToMutate.filter(x => x !== '').filter(x => x !== 0)
        while (rowToMutate.length !== 4) {
          direction === 'up' ?  rowToMutate.push('') : rowToMutate.unshift('');
        }
        for (let t = 0; t < 4; t++) {
          grid[t][i].value = rowToMutate[t]
        }
      }
    }
    fillRandomCell(grid)
  }

  const checkWinLossCondition = (grid) => {
    if (grid.some(x=> x.some(t => t.value === 2048))) {
      alert ('You won!')
      return 'stop'
    }
    for (let row = 0; row < 4; row++) {
    for (let col = 0; col < 4; col++) {
      if (grid[row][col] === 0) {
        return false; // Game continues (empty space exists)
      }
    }
  }
  
  // Check for possible merges
  for (let row = 0; row < 4; row++) {
    for (let col = 0; col < 4; col++) {
      const current = grid[row][col];
      
      // Check right neighbor
      if (col < grid[row].length - 1 && grid[row][col + 1] === current) {
        return false; // Possible merge exists
      }
      
      // Check bottom neighbor
      if (row < grid.length - 1 && grid[row + 1][col] === current) {
        return false; // Possible merge exists
      }
    }
  }
  }

  startGame()
  </script>

<template>
  <div class="wrapper bg-amber-50 h-screen w-screen p-3">
    <div class="game-container mx-auto px-10 w-150">
      <GameInfo :score="score" />
      <GameGrid :game-grid="gameGrid"/>
      <Controls @send-curr-direction="handleSentDirection"/>
    </div>
  </div>  
</template>

<style>
  * {
    box-sizing: border-box;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }
</style>


