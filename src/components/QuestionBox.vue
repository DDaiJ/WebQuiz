<template>
<div class="question-box-container">
	<b-jumbotron>
		<template v-slot:lead>
			{{ currentQuestion.question }}
		</template>

		<hr class="my-4">

		<b-container class="bv-example-row">
		<b-row>
			<b-col sm="8" offset="2">
				<b-list-group>
					<b-list-group-item
						v-for="(answer, index) in shuffledAnswers" :key="index"
						@click="selectAnswer(index)"
						:class="answerClass(index)"
					>
						{{ answer}}
					</b-list-group-item>
				</b-list-group>
			</b-col>
		</b-row>
		</b-container>

		
		<b-button 
			@click="checkAnswer" 
			variant="primary"
			:disabled="selectedIndex === null || submited"
		>
			Submit
		</b-button>
		<b-button @click="next" variant="success" :disabled="!submited">Next</b-button>
	</b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'

export default {
	props: {
		currentQuestion: Object,
		next: Function,
		increment: Function
	},
	data() {
		return {
			selectedIndex: null,
			shuffledAnswers: [],
			correctIndex: null,
			submited: false,
		}
	},
	computed: {
		answers() {
			// "...": making a copy of the target array
			let answers = [...this.currentQuestion.incorrect_answers]
			answers.push(this.currentQuestion.correct_answer)
			return answers
		}
	},
	// watch is run every time the target variable, e.g: currentQuestion
	// is updated
	watch: {
		currentQuestion: {
			// immediate is used so that handler is also run when
			// currentQuestion is first passed in
			immediate: true,
			handler() {
				this.selectedIndex = null
				this.shuffleAnswers()
				this.submited = false;
			}
		}

		// currentQuestion() {
		// 	this.selectedIndex = null
		// 	this.shuffleAnswers()
		// }
	},
	methods: {
		selectAnswer(index) {
			this.selectedIndex = index

		},
		checkAnswer() {
			let isCorrect = false;

			if (this.selectedIndex === this.correctIndex) {
				isCorrect = true
			}
			this.increment(isCorrect)
			this.submited = true;
		},
		shuffleAnswers() {
			let answers = [...this.currentQuestion.incorrect_answers]
			answers.push(this.currentQuestion.correct_answer)
			// gonna use lodash library to help shuffer the array
			this.shuffledAnswers = _.shuffle(answers)
			this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
		},
		answerClass(index) {
			if (!this.submited && this.selectedIndex === index) {
				return 'selected'
			}
			if (this.submited && this.correctIndex === index) {
				return 'correct'
			}
			if (this.submited && this.selectedIndex === index) {
				return 'incorrect'
			}
			return ''
		}
	}
}
</script>

<style scoped>

.list-group {
	margin-bottom: 15px;
}

.list-group-item:hover {
	background: #EEEEFF;
	cursor: pointer;
}

.btn {
	margin: 0 5px;
}

.selected {
	background-color: #EEEEFF;
}

.correct {
	background-color: rgb(100, 243, 87);
}
.incorrect {
	background-color: rgb(245, 50, 50);
}
</style>