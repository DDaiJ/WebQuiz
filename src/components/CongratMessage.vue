<template>
    <div>
        <b-jumbotron header="Congrats on finishing the quiz!">
            <br/>
            <h1>Your total score: {{correctNum}} / {{answered}}</h1>
            <br/>
            <b-button @click="tryAgain" variant="primary" href="#">Try Again</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
export default {
    props: {
        correctNum: null,
        answered: null,
        tryAgain: Function
    }
}
</script>