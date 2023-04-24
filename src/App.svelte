<script>
  import {data} from './data.json.js'
  import {onMount} from "svelte";
  import {current_component} from "svelte/internal";

  let main_data = data;

  $: current_question_number = 0;
  $: answer_id = 0;
  $: current_question = data.questions[current_question_number];

  function validateQuestion(){

  }

  function startQuiz(){
    current_question_number = 1;
    current_question = data[current_question_number];
  }

  function previousQuestion(){
    current_question_number = current_question_number - 1;
    current_question = data[current_question_number];
    answer_id = 0;
  }

  function nextQuestion(){
      current_question_number = current_question_number + 1;
      current_question = data[current_question_number];
      answer_id = 0;
  }

  function doubt(){
    // add "doubt": "true" to the current question and add it to the doubt.txt
    let temp = current_question;
    temp.doubt = true;

    // add the question to the doubt.txt
    let doubt_file = new File([""], "doubt.txt", {type: "text/plain"});
    doubt_file.append(temp);
  }

  function getId(){
    answer_id = answer_id + 1;
    return answer_id;
  }



</script>

<body>

  <div id="container">

    <div id="card">

      <p class="Title">Hi!</p>


      {current_question.question}

      {#each current_question.answers as answer}

        <button id={getId()} on:click={validateQuestion}>{answer.Answer}</button>

      {/each}

      <button on:click={previousQuestion()}>Previous</button>
      <button on:click={validateQuestion()  }>Validate</button>

    </div>

  </div>


</body>
<style>





</style>



