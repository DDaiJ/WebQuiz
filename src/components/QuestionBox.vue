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
						:class="[selectedIndex === index ? 'selected' : '']"
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
		>
			Submit
		</b-button>
		<b-button @click="next" variant="success">Next</b-button>
	</b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'

export default {
	props: {
		currentQuestion: Object,
		next: Function
	},
	data() {
		return {
			selectedIndex: null,
			shuffledAnswers: []
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
			if (this.shuffledAnswers[this.selectedIndex] === this.currentQuestion.correct_answer) {
				return true;
			}
			return false;	
		},
		shuffleAnswers() {
			let answers = [...this.currentQuestion.incorrect_answers]
			answers.push(this.currentQuestion.correct_answer)
			// gonna use lodash library to help shuffer the array
			this.shuffledAnswers = _.shuffle(answers)
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
	background-color: #20DD40;
}
.incorrect {
	background-color: #DD2020;
}
</style>