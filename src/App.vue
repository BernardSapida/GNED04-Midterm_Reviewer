<template>
  <main>
    <div class="mx-5 my-5 card">
        <div class="card-header d-flex justify-content-between align-items-center py-3">
            <h3 class="m-0">score: {{ score }} / {{ questions.length }}</h3>
        </div>
        <div class="card-body">
            <div v-if="message" class="text-white p-3 rounded mb-3" :class="[ message == 'correct' ? 'bg-success' : 'bg-danger' ]">
                <p class="m-0">{{ message == "correct" ? "Congratulation! Your answer is correct." : message }}</p>
            </div>
            <div class="mb-3">
                <label for="answer" class="form-label"><strong class="text-danger fs-2">Q.</strong> <span class="question fs-3">{{ question }}</span></label>
                <input 
                    type="text" 
                    class="form-control mt-2" 
                    v-model="user_answer" 
                    placeholder="Type your answer" 
                    aria-label="answer" 
                    @keyup.enter="verifyAnswer()" 
                    :disabled="isSubmitted">
            </div>
        </div>
        <div class="card-footer d-flex justify-content-end align-items-center py-3">
            <button v-if="!isSubmitted" class="btn btn-primary" @click="verifyAnswer()">Submit Answer</button>
            <button v-else class="btn btn-success" @click="nextQuestion()">Next Question</button>
        </div>
    </div>
  </main>
</template>

<script>
    export default {
        mounted() {
            this.questions = this.questions.sort((a, b) => 0.5 - Math.random());
            this.nextQuestion();
        },
        data() {
            return {
                count: 0,
                message: null,
                question: "",
                answer: "",
                user_answer: "",
                isSubmitted: false,
                currentIndex: 0,
                score: 0,
                questions: [
                  {
                    question: "It is signed on December 15, 1987, created a negotiation",
                    answer: "Unstructured Data/Multimedia Data"
                  }
                ]
            }
        },
        methods: {
            verifyAnswer() {
                if(this.user_answer == "") this.message = "Please put your answer";
                else {
                    if(this.user_answer.trim().toLowerCase() == this.answer.toLowerCase()) {
                        this.message = "correct";
                        this.score++;
                    } else this.message = "The correct answer is: " + this.answer;

                    this.isSubmitted = true;
                }
            },
            nextQuestion() {
                this.isSubmitted = false;
                this.message = null;
                this.user_answer = "";

                if(this.currentIndex == this.questions.length) {
                    this.currentIndex = 0;
                    this.score = 0;
                    this.questions = this.questions.sort(() => 0.5 - Math.random());
                }

                const SELECTED_QUESTION = this.questions[this.currentIndex];
                const QUESTION = `${this.currentIndex + 1}) ${SELECTED_QUESTION.question}`;
                const ANSWER = SELECTED_QUESTION.answer;

                this.question = QUESTION;
                this.answer = ANSWER;

                this.currentIndex++;
            }
        }
    }
</script>