<template>
    <div class="header">
            <b-nav tabs>
                <b-nav-item disabled>Computer Trivia Quiz</b-nav-item>
                <b-nav-item>Question No: {{ itemNo+1 }}</b-nav-item>
                <b-nav-item disabled>No. Correct Items: {{ numCorrect }}/{{ totalAnswered }}</b-nav-item>
                <b-nav-item disabled>Total Items: 10</b-nav-item>
            </b-nav>
    </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'totalAnswered',
        'itemNo'
    ]
}
</script>