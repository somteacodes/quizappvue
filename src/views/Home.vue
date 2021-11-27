/* eslint-disable prettier/prettier */
<template>
  <main class="flex h-screen items-center justify-center bg-gray-100">
    <!-- quiz overlay -->
    <QuizCompleteOverlay
      v-if="endOfQuiz"
      :percent="percentageScore"
      @restartQuiz="onQuizStart"
    ></QuizCompleteOverlay>

    <!-- quiz container -->

    <div
      class="overflow-hidden bg-white flex-none container relative shadow-lg rounded-lg px-12 py-6"
    >
      <img
        src="@/assets/images/abstract.svg"
        alt=""
        class="absolute -top-10 left-0 object-none"
      />

      <!-- contents -->
      <div class="relative z-20">
        <!-- score container -->
        <div class="text-right text-gray-800">
          <p class="text-sm leading-3">Score</p>
          <p class="font-bold">{{ score }}</p>
        </div>

        <!-- timer container -->
        <div class="bg-white shadow-lg p-1 rounded-full w-full h-5 mt-4">
          <div
            class="bg-blue-700 rounded-full w-11/12 h-full"
            :style="`width:${timer}%`"
          ></div>
        </div>

        <!-- question container -->
        <div
          class="rounded-lg bg-gray-100 p-2 neumorph-1 text-center font-bold text-gray-800 mt-8"
        >
          <div class="bg-white p-5">
            {{ formattedQuestion }}
          </div>
        </div>

        <!-- options container -->

        <div class="mt-8">
          <!-- option container -->

          <div v-for="(choice, item) in currentQuestion.choices" :key="item">
            <div
              class="neumorph-1 option-default bg-gray-100 p-2 rounded-lg mb-3 relative"
              :ref="optionChosen"
              @click="onOptionClicked(choice, item)"
            >
              <div
                class="bg-blue-700 p-1 transform rotate-45 rounded-md h-10 w-10 text-white font-bold absolute right-0 top-0 shadow-md"
              >
                <p class="transform -rotate-45">+10</p>
              </div>

              <div class="rounded-lg font-bold flex p-2">
                <!-- option ID -->
                <div class="p-3 rounded-lg">{{ item }}</div>

                <!-- option name -->
                <div class="flex items-center pl-6">{{ choice }}</div>
              </div>
            </div>
          </div>
        </div>

        <!-- progress indicator container -->
        <div class="mt-8 text-center">
          <div class="h-1 w-12 bg-gray-800 rounded-full mx-auto"></div>
          <p class="font-bold text-gray-800">
            {{ questionCounter }}/{{ questions.length }}
          </p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.neumorph-1 {
  box-shadow: 6px 6px 18px rgba(0, 0, 0, 0.09), -6px -6px 18px #ffffff;
}
.container {
  max-width: 400px;
  border-radius: 25px;
}
</style>

<script>
import { onMounted, ref } from "vue";
import QuizCompleteOverlay from "./components/QuizCompleteOverlay";
export default {
  setup() {
    // data
    let canClick = true;
    let timer = ref(100);
    let endOfQuiz = ref(false);
    let questionCounter = ref(0);
    let score = ref(0);
    const currentQuestion = ref({
      question: "",
      answer: 1,
      choices: [],
    });
    let percentageScore = ref(0);

    const questions = ref([]);

    const loadQuestion = () => {
      canClick = true;
      // Check if there are more questions to load
      if (questions.value.length > questionCounter.value) {
        // load question
        timer.value = 100;
        currentQuestion.value = questions.value[questionCounter.value];
        console.log("Current questions", currentQuestion.value);
        questionCounter.value++;
      } else {
        // no more questions
        onQuizEnd();
        console.log("Out of questions");
      }
    };

    // methods/functions
    let itemsRef = [];
    const optionChosen = (element) => {
      if (element) {
        itemsRef.push(element);
      }
    };

    const clearSelected = (divSelected) => {
      setTimeout(() => {
        divSelected.classList.remove("option-correct");
        divSelected.classList.remove("option-wrong");
        divSelected.classList.add("option-default");
        loadQuestion();
      }, 1000);
    };

    const onOptionClicked = (choice, item) => {
      if (canClick) {
        const divContainer = itemsRef[item];
        const optionID = item + 1;
        if (currentQuestion.value.answer == optionID) {
          console.log("you are correct");
          score.value += 10;
          divContainer.classList.add("option-correct");
          divContainer.classList.remove("option-default");
        } else {
          console.log("you are wrong");
          divContainer.classList.add("option-wrong");
          divContainer.classList.remove("option-default");
        }
        timer.value = 100;
        canClick = false;
        // TODO go to next question
        clearSelected(divContainer);
        console.log(choice, item);
      } else {
        // Cant select option
        console.log("cant select question");
      }
    };

    const countDownTimer = function() {
      let interVal = setInterval(() => {
        if (timer.value > 0) {
          timer.value--;
        } else {
          console.log("timer is up");
          onQuizEnd();
          clearInterval(interVal);
        }
      }, 150);
    };

    const fetchQuestionsFromServer = async function() {
      console.log("fetch questions from server");
      fetch("https://opentdb.com/api.php?amount=10&category=18")
        .then((res) => {
          return res.json();
        })
        .then((data) => {
          // map json to fit our own arrangement
          const newQuestions = data.results.map((serverQuestion) => {
            const arrangedQuestion = {
              question: serverQuestion.question,
              choices: "",
              answer: "",
            };

            const choices = serverQuestion.incorrect_answers;

            arrangedQuestion.answer = Math.floor(Math.random() * 4 + 1);

            choices.splice(
              arrangedQuestion.answer - 1,
              0,
              serverQuestion.correct_answer
            );

            arrangedQuestion.choices = choices;

            return arrangedQuestion;
          });
          console.log("new formatted questions", newQuestions);
          questions.value = newQuestions;
          loadQuestion();
          countDownTimer();
        });
    };

    const onQuizEnd = function() {
      // calculate the percentage
      percentageScore.value = (score.value / 100) * 100;

      // stop timers
      timer.value = 0;

      // show overlay
      endOfQuiz.value = true;
    };

    const onQuizStart = function() {
      //1 . set default values
      canClick = true;
      timer.value = 100;
      endOfQuiz.value = false;
      questionCounter.value = 0;
      score.value = 0;
      currentQuestion.value = {
        question: "",
        answer: 1,
        choices: [],
      };
      percentageScore.value = 0;
      questions.value = [];

      //2. fetch questions from server
      fetchQuestionsFromServer();
    };

    // lifecycle hooks
    onMounted(() => {
      fetchQuestionsFromServer();
    });
    // return
    return {
      timer,
      currentQuestion,
      questions,
      score,
      questionCounter,
      loadQuestion,
      onOptionClicked,
      optionChosen,
      endOfQuiz,
      onQuizEnd,
      percentageScore,
      onQuizStart,
    };
  },

  computed: {
    formattedQuestion() {
      let entities = {
        amp: "&",
        apos: "'",
        "#x27": "'",
        "#x2F": "/",
        "#39": "'",
        "#47": "/",
        lt: "<",
        gt: ">",
        nbsp: " ",
        quot: '"',
        "#039": "'",
      };

      return this.currentQuestion.question.replace(/&([^;]+);/gm, function(
        match,
        entity
      ) {
        return entities[entity] || match;
      });
    },
  },
  components: {
    QuizCompleteOverlay,
  },
};
</script>
