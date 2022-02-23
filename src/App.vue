<script setup>
import { computed, ref } from 'vue';
//--------- PART - Word -------------
//คำที่มีในเกมนี้
const words = [
  {word: 'house', hint:'A place where you live'},
  {word: 'java', hint: 'A name of programming language'},
  {word: 'iphone', hint: "A popular phone's brand"},
  {word: 'earth', hint: 'the star that we live'},
  {word: 'salmon', hint: 'Name of a fish'},
  {word: 'plane', hint: 'A vehicla that can fly'},
  {word: 'thailand', hint: 'Siam, Land of smile'},
  {word: 'guitar', hint: 'An instrument'},
  {word: 'hangman', hint: 'Game we are playing'}
]

//คำที่สุ่มมา
const randomWords = computed(() => {
  return words[Math.floor(Math.random() * words.length)]
})
//ดึง hint ออกมาจากคำที่สุ่ม
const hint = ref(randomWords.value.hint)
//ดึง word ออกมาจากคำที่สุ่ม และแยกเป็น array ช่องละตัวอักษร
const arrayOfWord = ref(randomWords.value.word.split(''))

console.log(randomWords.value)
console.log(randomWords.value.word.length)
console.log(arrayOfWord.value)


//--------- PART - Letter -------------
//ตัวอักษรทั้งหมด เก็บเป็น array
const letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 
                'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z']
//ชีวิต เริ่มที่ 10 
const lives = ref(10)

//function ที่กดแล้ว ปุ่มจะdisable + เช็ค hangman
//ตัวอักษรที่เลือกกแล้ว เก็บเป็น array
const chosenChar = ref([])
//function ที่ทำงานเมื่อคลิ๊ก
const clickLetter = (letter) => {
    if(lives.value !== 0){
      chosenChar.value.push(letter)
    let thisLetter = document.getElementById(letter)
    thisLetter.disabled  = true
    // เช็ค hangman
    if(!arrayOfWord.value.includes(letter)){
      lives.value--
    }
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

//funtion ที่เช็คว่า win รึยัง
//check ว่าตัวอักษรที่เลือกมา มีอยู่ในคำที่สุ่มครบรึยัง
const checkCorrect = computed(() => {
    return arrayOfWord.value.every(letter => chosenChar.value.includes(letter))
})

const images = [
  '../public/hangmans/hangman-0.png',
  '../public/hangmans/hangman-1.png',
  '../public/hangmans/hangman-2.png',
  '../public/hangmans/hangman-3.png',
  '../public/hangmans/hangman-4.png',
  '../public/hangmans/hangman-5.png',
  '../public/hangmans/hangman-6.png',
  '../public/hangmans/hangman-7.png',
  '../public/hangmans/hangman-8.png',
  '../public/hangmans/hangman-9.png',
  '../public/hangmans/hangman-10.png',
]

</script>

<template>
  <h1>Hangman Game</h1>
    
    <!-- แถบ You have Lives -->
    <div>
      <p v-if="lives!==0">You have {{ lives }} lives</p>
      <h3 v-else>Game Over!! <br> The answer is {{ randomWords.word }}</h3>
      <h3 v-show="checkCorrect">You Win!!!</h3>
    </div>

    <br>

    <!-- แถบคำที่ random มา -->
      <div>
        <span v-for="(letter,index) in arrayOfWord" :key="index" class="block">
          <span v-if="chosenChar.includes(letter)==true">{{ letter }}</span>
          <span v-else>_</span>
        </span>
      </div>
      
   <br>
   
    <img :src="images[lives]" width="250">
    
  <!-- แป้นพิมตัวอักษรทั้งหมด -->
  <div class="col-4">
    <button v-for="(letter, index) in letters" :key="index" 
            @click="clickLetter(letter)"
            :id="letter"
            class="letter-button"
            :disabled="checkCorrect">{{ letter }}</button>
  </div>
  
<!-- แถบปุ่ม hint + play -->
  <div class="action-button">
    <button @click="showHint" v-show="lives <= 5" :disabled="hintStatus">Hint</button>
    <span v-show="hintStatus" class="hint">{{ hint }}</span>

    <span class="playAgain">
    <button @click="playAgain"  >Play Again</button>
    </span>
  </div>

</template>


<style>

  @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');
  /* @font-face {
    font-family: "fc-pallete-color";
    src:(../assets/fonts/FC-Pallete-Color.ttf), format("truetype");
  } */
  body {
    font-family: 'Press Start 2P', cursive;
    padding: 2em;
    margin: 2em;
    background-color: lightpink;
    text-align: center;
    /* font-family: "fc-pallete-color"; */
    line-height: 30px;
  }
  button {
    font-family: 'Press Start 2P', cursive;
    border-radius: 10px;
    padding: 10px;
    margin: 4px;
    border: 3px #000;
  }
  button:hover:not([disabled]) {
	background-color: lightcoral;
  color: white;
	transition-duration: .5s;
  }
  button:active:not([disabled]){
    background-color: white;
    transition-duration:2s;
  }
  .hint {
    padding: 30px;
  }
  .playAgain {
    margin: 2em;
    position: fixed;
    top: 0;
    right: 0;
    padding: 30px;
  }
  .letter-button{
    padding: 16px;
    height: 60px;
    width: 60px;
    font-size: 20px;
  }
  .action-button {
  padding: 16px;
  margin : 6px;
  /* display: flex; */
  text-align: left;
  height: 60px;
  gap: 5px;
  }
  /* .letter { 
    color: blueviolet;
  } */
  .block {
    padding: 12px;
    margin : 8px;
    width: 52px;
    height: 52px;
    border: 3px solid;
    border-radius: 15px;
    border: lightcoral solid thick;
    color: black;
    text-align: center;
    font-size: 24px;
  }

</style>
