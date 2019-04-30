<template>
    <div class="container">
        <div class="row justify-content-center">

            <div class="col-md-12">
                <div class="card card-default">

                    <div class="card-header">
                        Vue Js Draggable and Timer Quiz
                        <div v-if="isActiveClass !== 5" class="vue-timer-section">
                            <vue-countdown-timer
                                @start_callback="startCallBack('event started')"
                                @end_callback="endCallBack('event ended')"
                                :start-time="startDateNew"
                                :end-time="endDateNew"
                                :interval="1000"
                                :start-label="'Timer '"
                                :end-label="'Timer '"
                                :end-text="'Timer : 0 second'"
                                :seconds-txt="' seconds'">
                                <template slot="countdown" slot-scope="scope">
                                    <span>{{scope.props.seconds}}</span><i>{{scope.props.secondsTxt}}</i>
                                </template>
                            </vue-countdown-timer>
                        </div>
                    </div>
                    <div class="card-body">
                        <span v-if="isActiveClass !== 5" class="true-section">
                            <label class="true-label">Yes</label>
                        </span>
                        <span v-if="isActiveClass !== 5" class="false-section">
                            <label class="false-label">No</label>
                        </span>

                        <drag-it-dude
                            @dragging="handleDragging"
                            @dropped="handleDropped"
                            v-for="item in exampleList" :key="item.id"
                            v-bind:style="{ left: '485px', top: '350px' }"
                            >
                            <div v-if="isActiveClass === item.id" class="innerElement">{{ item.text }}</div>
                        </drag-it-dude>
                        <div v-if="isActiveClass === 5" class="score-text">Your score is = {{score}}</div>
                        <div v-if="isActiveClass === 5" class="score-text">Total True Answers = {{trueAnswer}}</div>
                        <div v-if="isActiveClass === 5" class="score-text">Total False Answers = {{4 - trueAnswer}}</div>
                        <button v-if="isActiveClass === 5" class="button" @click="reloadPage">Restart Quiz</button>

                    </div>

                    <div class="card-header footer-section-text">
                        Drag the question box to Left side if statement is Correct and Right side if statement is Incorrect
                    </div>

                </div>
            </div>

        </div>
    </div>
</template>

<script>

    import DragItDude from "vue-drag-it-dude";

    export default {
        name: "App",
        components: {
            DragItDude
        },
        data() {
            return {
                exampleList: [
                    { id: 1, text: "10 * 10 = 100 ?" },
                    { id: 2, text: "3 * 3 = 6 ?" },
                    { id: 3, text: "10 + 90 = 100 ?" },
                    { id: 4, text: "100 * 100 = 200 ?" }
                ],
                isActiveClass: 1,
                score: 0,
                startDateNew: Date.now(),
                endDateNew: Date.now() + 11000,
                trueAnswer: 0
            }
        },
        methods: {
            handleDragging() {
                this.text = "That's place is awesome!";
            },
            handleDropped() {
                this.text = "Drag me!";

                var size = parseInt(event.target.style.left, 10);
                if(size != '' && size < 300){

                    if(this.isActiveClass == 1){
                        this.score = this.score + 10;
                        this.trueAnswer = this.trueAnswer + 1;
                    } else if(this.isActiveClass == 3){
                        this.score = this.score + 10;
                        this.trueAnswer = this.trueAnswer + 1;
                    }

                    this.isActiveClass = this.isActiveClass + 1;
                }
                if(size != '' && size > 800){

                    if(this.isActiveClass == 2){
                        this.score = this.score + 10;
                        this.trueAnswer = this.trueAnswer + 1;
                    } else if(this.isActiveClass == 4){
                        this.score = this.score + 10;
                        this.trueAnswer = this.trueAnswer + 1;
                    }

                    this.isActiveClass = this.isActiveClass + 1;
                }
            },
            reloadPage(){
                window.location.reload();
            },
            startCallBack: function (x) {

            },
            endCallBack: function (x) {
                this.isActiveClass = 5;
            },
        }
    };
</script>
