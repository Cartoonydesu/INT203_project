<script setup>
import { computed, ref } from 'vue';
//--------- PART - Word -------------
const words = [
  {word: 'home', hints:'A place where you live'},
  {word: 'java', hints: 'A name of programming language'},
  {word: 'iphone', hints: "A popular phone's brand"},
  {word: 'world', hints: 'the star that we are living'}
]

const randomWords = computed(() => {
  return words[Math.floor(Math.random() * words.length)]
})
const hint = ref(randomWords.value.hints)
const arrayWord = ref(randomWords.value.word.split(''))

console.log(randomWords.value)
console.log(randomWords.value.word.length)
console.log(arrayWord.value)


//--------- PART - Letter -------------
const letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 
                'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z']
const life = ref(10)

//function ที่กดแล้ว ปุ่มจะdisable + เช็ค hangman
const chosenChar = ref([])//////////
const indexLetter = ref(-1)
const clickLetter = (letter) => {
  if(life.value !== 0){
    chosenChar.value.push(letter)
    let thisLetter = document.getElementById(letter)
    thisLetter.disabled  = true
    // เช็ค hangman
    if(!arrayWord.value.includes(letter)){
      life.value--
    }
    // if(!chosenChar.value.includes(letter)){
    //   life.value-- 
    // }
  }
}


//function ที่กดแล้วจะ show hint
const hintStatus = ref(false)
const showHint = () => {
  hintStatus.value = true
}

//function ในการอ่ะ รีๆๆๆๆๆๆๆๆ
const playAgain = () => {
  location.reload()
}
//To do list!=-=-=-=-=-=-=-=-=--=-=-=-=-=-=-=-=
//เฉลยตอนแพ้
//ทำ you win
//ภาพ hang man
//UX UI
//live เหลือ 5 ค่อยให้ hint
//(ต่อยอด)กดแป้มพิมได้
//เพิ่ม word แบบไม่มีเว้นวรรค*
//------------------


</script>





<template>
  <h1>Hangman Game</h1>
  <div class="action-button">
    <button v-for="(letter, index) in letters" :key="index" 
            @click="clickLetter(letter)"
            :id="letter">{{ letter }}</button>
  </div>
  <div>
    <p v-if="life!==0">You have {{ life }} lives</p>
    <p v-else>Game Over!!</p>
  </div>
  <div>
    <span v-for="(letter,index) in arrayWord" :key="index" class="block">
      <span v-if="chosenChar.includes(letter)==false">_</span>
      <span v-else>{{ letter }}</span>
    </span>
  </div>
  <div class="action-button">
    <button @click="showHint">Hint</button>
    <div v-show="hintStatus">{{ hint }}</div>
    <button @click="playAgain">Play Again</button>
  </div>
</template>





<style>
  body {
    padding: 2em;
    margin: 2em;
    background-color: lightpink;
    text-align: center;
  }
  button {
    border-radius: 5px;
    padding: 10px;
    margin: 3px;
  }
  .action-button {
  padding: 16px;
  margin : 6px;
  display: flex;
  gap: 5px;
  }
  .letter { 
    color: blueviolet;
  }
  .block {
    padding: 10px;
    margin : 10px;
    width: 30px;
    height: 30px;
    border: 5px solid;
    border-radius: 10px;
    border-color: lightcoral;
    color: black;
    text-align: center;
  }
</style>


