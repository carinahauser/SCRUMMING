<script>
  import {data} from './data.json.js'
  import {onMount} from "svelte";



  let main_data = data;

  $: current_question_number = 0;
  let answer_id = 0;
  $: current_question = data.questions[current_question_number];
  $: correct_ones = 0;
  $: correct = [];
  $: evaluate = true


  let last_question = 0;



  function validateQuestion(){
    // for the amount of answers in the current question check, if the answer is correct
    for (let i = 0; i < current_question.answers.length; i++){
      // if the answer is correct, add correct as a class to the DOM
        if (current_question.answers[i].correct === "True"){
          let temp = document.getElementById("qes" + (i+1).toString())
          console.log(temp)
          temp.classList.add('correct')
        }
        else  {
          console.log(i+1)
          let temp = document.getElementById("qes" + (i+1).toString())
          console.log(temp)
          temp.classList.add('incorrect')
        }
    }
    evaluate = false;
  }

  function clearClasses(){
    for (let i = 0; i < current_question.answers.length; i++){
      let temp = document.getElementById("qes" + (i+1).toString())
      temp.classList.remove('correct')
      temp.classList.remove('incorrect')
      temp = document.getElementById("r" + (i+1).toString())
      temp.checked = false;
    }
  }

  onMount(() => {
    correct_ones = getAmount_of_correct();
  });

  function startQuiz(){
    current_question_number = 0;
    current_question = data[current_question_number];
    correct_ones = getAmount_of_correct();
  }

  function randomNext(){
    last_question = current_question_number;
    answer_id = 0;

    clearClasses();
    current_question_number = Math.floor(Math.random() * data.questions.length);
    current_question = data.questions[current_question_number];

    correct_ones = getAmount_of_correct()
    correct = [];
    evaluate = true;
  }


  function previousQuestion(){
    clearClasses();
    answer_id = 0;
    current_question_number = last_question;
    current_question = data.questions[current_question_number];
    correct_ones = getAmount_of_correct()
    correct = [];
    evaluate = true;
    alert("You can't go back in this quiz. You can only go forward. Sorry :(")
  }

  function getAmount_of_correct(){
    // get amount of answers where "correct": true for current_question

    let amount = 0;
    for (let i = 0; i < current_question.answers.length; i++){
      if (current_question.answers[i].correct === "True"){
        amount = amount + 1;
      }
    }
    return amount;

  }

  function getId(next){
    if (next){
      answer_id = answer_id + 1;
    }
    return answer_id;
  }





</script>


<div class=" h-100 d-flex justify-content-center align-items-center" id="background">
  <div class="card bg-dark text-white" id="card">
    <div class ="card-header">
      <h3>Quiz</h3>
    </div>

    <div class="quiz_wrapper card-body h-100">
      <div class = "w-75 align-content-center align-self-center">
        <div id = "num" class="is-align-items-self-end">Question Number: {current_question_number} </div>
        <div id="quiz-question" class="w-100 align-content-center align-self-center"> {current_question.question} </div>
        <div id="hint_on_correct" class="w-75 align-content-center align-self-center">{correct_ones} answers are correct. </div>
      </div>
      {#key current_question }
      <div id="quiz-answers" class="d-flex flex-column justify-content-center align-items-center">
        <div id="checkboxes" class="w-75 align-content-center align-self-center">
          {#each current_question.answers as answer}
            <div class="whatever ">
              <div class="answer_number text-center">{getId(true)}</div>
              <input type="checkbox" name="rGroup" value="1" id="r{getId()}" />
              <label class="Answer answer_text" id="qes{getId()}" for="r{getId()}"> {answer.Answer}</label>
            </div>
          {/each}
        </div>
      </div>
        {/key}
    </div>

    <div id="buttons_bottom" class="quiz-control w-75 align-self-center align-self-end">
      <div class="row align-self-end">
        <div class="col align-self-end">
          <button class="btn-primary align-self-end btn w-100" on:click={previousQuestion}>Previous </button>
        </div>
        <div class="col">
          <!-- space for more :) -->
        </div>
        <div class="col">
          {#if evaluate}
            <button class="btn-primary align-self-end btn w-100"  on:click|once={validateQuestion} >Evaluate </button>
          {:else }
            <button class="btn-primary align-self-end btn w-100" on:click|once={randomNext}>Next </button>
          {/if}

        </div>
      </div>

    </div>

  </div>


</div>



<style>





</style>



