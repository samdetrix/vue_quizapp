<template>
    <div tabs>
        <b-nav>
            <b-nav-item disabled>Quiz App</b-nav-item>
            <b-nav-item disabled>Count: {{ numCorrect }}/{{ numTotal }}</b-nav-item>    
        </b-nav>
    </div>
</template>
<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
    props:[
        'numCorrect',
        'numTotal'
    ]
})
</script>
