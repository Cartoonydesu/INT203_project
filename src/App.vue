<script setup>
import { computed, ref } from 'vue';
//--------- PART - Word -------------
//คำที่มีในเกมนี้
const words = [
  {word: 'home', hints:'A place where you live'},
  {word: 'java', hints: 'A name of programming language'},
  {word: 'iphone', hints: "A popular phone's brand"},
  {word: 'earth', hints: 'the star that we are living'},
  {word: 'salmon', hint: 'Name of a fish'},
  {word: 'plane', hint: 'A vehicla that can fly'},
  {word: 'thailand', hint: 'Siam, Land of smile'},
  {word: 'guitar', hint: 'An instrument'}
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
    animate()
    canvas()
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

//To do list!=-=-=-=-=-=-=-=-=--=-=-=-=-=-=-=-=
//เฉลยตอนแพ้ //
//ทำ you win //
//ภาพ hang man
//UX UI
//live เหลือ 5 ค่อยให้ hint //
//(ต่อยอด)กดแป้มพิมได้
//เพิ่ม word แบบไม่มีเว้นวรรค*
//------------------

var animate = function () {
    var drawMe = lives
    drawArray[drawMe]()
}

const canvas = ()=> {
    myStickman = document.getElementById("stickman")

    context = myStickman.getContext('2d')
    context.beginPath()
    context.strokeStyle = "#fff"
    context.lineWidth = 2

}

head = function(){
    myStickman = document.getElementById("stickman")

    context = myStickman.getContext('2d')
    context.beginPath()
    context.arc(60, 25, 10, 0, Math.PI*2, true)
    context.storke()
}
draw = function($pathFromx, $pathFromy, $pathTox, $pathToy){
    context.moveTo($pathFromx, $pathFromy)
    context.lineTo($pathTox, $pathToy)
    context.storke()
}
frame1 = function(){
    draw (0, 150, 150, 150)
}
frame2 = function(){
    draw (10, 0, 10, 600)
}
frame3 = function(){
    draw (0, 5, 70, 5)
}
frame4 = function(){
    draw (60, 5, 60, 15)
}
torso = function(){
    draw (60, 36, 60, 70)
}
rightArm = function(){
    draw (60, 46, 100, 50)
}
leftArm = function(){
    draw (60, 46, 20, 50)
}
rightLeg = function(){
    draw (60, 70, 100, 100)
}
leftLeg = function(){
    draw (60, 70, 20, 100)
}
drawArray = [rightLeg, leftLeg, rightArm, leftArm, torso, head ,frame4, frame3, frame2, frame1]
</script>





<template>
  <h1>Hangman Game</h1>
   <!-- <canvas id="stickman">This Text will show if the Browser does NOT support HTML5 Canvas tag</canvas> -->
    
    <!-- แทบ You have Lives -->
    <div>
      <p v-if="lives!==0">You have {{ lives }} lives</p>
      <h3 v-else>Game Over!! <br> The answer is {{ randomWords.word }}</h3>
      <h3 v-show="checkCorrect">You Win!!!</h3>
    </div>

    <br>

    <!-- แทบคำที่ random มา -->
      <div>
        <span v-for="(letter,index) in arrayOfWord" :key="index" class="block">
          <span v-if="chosenChar.includes(letter)==true">{{ letter }}</span>
          <span v-else>_</span>
        </span>
      </div>
      
   <br>
   <br>
    
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
    <button @click="showHint" v-show="lives <= 5">Hint</button>
    <br>
    <div v-show="hintStatus">{{ hint }}</div>
    <br>
    <button @click="playAgain">Play Again</button>
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
  }
  button {
    border-radius: 10px;
    padding: 10px;
    margin: 4px;
  }
  .letter-button{
    padding: 16px;
    height: 60px;
    width: 60px;
    font-size: 20px
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
    padding: 10px;
    margin : 10px;
    width: 30px;
    height: 30px;
    border: 3px solid;
    border-radius: 10px;
    border-color: lightcoral;
    color: black;
    text-align: center;
  }


  canvas{
  color: $white;
  border: #fff dashed 2px;
  padding:15px;
}
</style>


