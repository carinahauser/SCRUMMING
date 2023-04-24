<script>
  import {data} from './data.json.js'

  $: current_question_number = 0;
  $: current_question = data[current_question_number];

  function validateQuestion(){

  }

  function startQuiz(){
    current_question_number = 1;
    current_question = data[current_question_number];
  }

  function nextQuestion(){
      current_question_number = current_question_number + 1;
      current_question = data[current_question_number];
  }






</script>

<body>

  <div id="container">

    <div id="card">



    </div>

  </div>


</body>
<style>



</style>



