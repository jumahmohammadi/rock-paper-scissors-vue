<script setup>
    import {ref, computed, onMounted} from 'vue'
    const wins= ref(0) 
    const draws= ref(0) 
    const losses= ref(0)

    const choice = ref(null)
    const computerChoice=ref(null)
    const verdict = ref(null)
    const status= ref(0)

    const outcomes = {
        rock:{
            rock:'draw',    
            paper:'loss',    
            scissors:'win',    
         },

        paper:{
            rock:'win',    
            paper:'draw',    
            scissors:'loss',    
         },

         scissors:{
            rock:'loss',    
            paper:'win',    
            scissors:'draw',    
         }       
    } 

    const windPercentage=computed (()=>{
      const total= wins.value+draws.value+losses.value;
       return total ? (wins.value/total)*100 : 0
    })

    const play = c=>{
      choice.value=c

      const choices= ['rock','paper','scissors']
      const random=Math.floor(Math.random()*choices.length)
    
      computerChoice.value=choices[random];
      const outcome=outcomes[c][computerChoice.value]
   
        if(outcome==='win'){
           wins.value++;
           verdict.value="You Win!"
           status.value=1
        }else if(outcome==='loss'){
          losses.value++;
          verdict.value="You Lose!"
          status.value=-1
        }else{
          draws.value++;
          verdict.value="It is a Draw!"
          status.value=2
        }
        
        if(status==1){

        }else if(status==-1){

        }else{

        }

        SaveGame()
    }

    const SaveGame = ()=>{
      localStorage.setItem('wins',wins.value)
      localStorage.setItem('draws',draws.value)
      localStorage.setItem('losses',losses.value)
    }

    const loadGame = () =>{
      wins.value= parseInt(localStorage.getItem('wins',wins.value)) || 0
      draws.value=parseInt(localStorage.getItem('draws',draws.value)) || 0
      losses.value=parseInt(localStorage.getItem('losses',losses.value)) || 0
    }


    const ResetRound=()=>{
      choice.value=null
      computerChoice.value=null
      verdict.value=null

    }

    const ResetAll=()=>{
      choice.value=null
      computerChoice.value=null
      verdict.value=null

       wins.value=0
       draws.value=0
       losses.value=0

      localStorage.setItem('wins',0)
      localStorage.setItem('draws',0)
      localStorage.setItem('losses',0)

    }

    onMounted(()=>{
      loadGame()
      window.addEventListener('keypress;', e=>{
         if(e.key==='r'){
            // ResetRound()
         }
      })
    })
</script>

<template>
   <div class="bg-gray-700 text-white text-center min-h-screen flex flex-col">
       <header class="container mx-auto p-6">
           <h1 class="text-4xl font-bold">
               Rock, Paper Scissors
           </h1>
       </header>

       <main class="container mx-auto p-6 flex-1">
           <div v-if="choice===null" class="flex items-center justify-center -mx-6">
              <button @click="play('rock')" class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-pink-500">
                 <img src="./assets/rock.svg" alt="Rock" class="w-full " />
              </button>

                <button @click="play('paper')" class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-green-500">
                 <img src="./assets/paper.svg" alt="Paper" class="w-full " />
              </button>

                <button @click="play('scissors')" class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-yellow-500">
                 <img src="./assets/scissors.svg" alt="Scissors" class="w-full " />
              </button>

           </div>
           <div v-else>
            <div class="boxes">
              <div class="text-3xl mb-4 text-center box">
                 <span class="text-green-500">You</span>
                 <img :src="`src/assets/${choice}.svg`"  class="mx-auto" >
              </div>
               <div class="text-3xl mb-4 text-center box">
                 <span class="text-pink-500">Computer</span>
                 <img :src="`src/assets/${computerChoice}.svg`"  class="mx-auto">
              </div>
            </div>

              <div class="text-6xl my-4 " :class=" status===1 ? 'text-green-500' : status===-1  ?   'text-red-500' : 'text-orange-500' ">
                  {{ verdict }}  <img style="display:inline;width:70px" :src=" status===1 ? 'src/assets/win.png' : status===-1  ?   'src/assets/lose.png' : 'src/assets/draw.png' ">
              </div>
              <button @click="ResetRound" class="bg-pink-500 text-lg py-2 px-4">Reset</button>
           </div>

           <div class="mt-12 text-3xl mb-4">
             {{ wins }} :{{ draws }} : {{ losses }}
           </div>
           <div class="text-lg">
             Win rate: {{ Math.round(windPercentage) }} %
           </div> 
           <div class="mb-4">
              <button @click="ResetAll" class="bg-blue-500 text-lg py-2 px-4 mt-12 rounded-4 ">Reset All</button>
           </div>
       </main>
   </div> 
</template>

<style scoped>
  .boxes{
    display:flex;
    justify-content: space-around;
  }
  .boxes .box{
      border: 1px solid gray;
      padding: 20px;
  }
  .boxes .box img{
    width:10rem; 
  }
</style>
