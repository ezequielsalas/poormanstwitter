<template>
    <div class="tweet-form-container">
        <div class="row-input">
            <div class="form-control">
                <label for="name">Name</label>
                <input v-model="userName" class="input-style input-inline" type="text" id="name"/>
            </div>

            <div class="form-control">
                <label for="tweet">Message <span v-bind:class="[tweetMessage.length >= 50 ? 'danger' : '']">{{tweetMessage.length}}/50</span>
                </label>
                <textarea maxlength="50" class="input-style" v-model="tweetMessage" rows="4" cols="50" id="tweet"
                          placeholder="What's happening?">
                        </textarea>
            </div>
        </div>

        <div class="row-button">
            <div class="form-control">
                <button class="tweet-button" v-on:click="saveTweet"> Tweet</button>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'TweetForm',
        methods: {
            saveTweet: function () {

                let globalThis = this;
                console.log(globalThis)
                window.jQuery.ajax({contentType: 'application/json',type: 'POST',
                    url: 'http://localhost:8000/',    data:JSON.stringify({name: this.userName, message: this.tweetMessage,datetime:  new Date()}), success: function(data){
                        globalThis.$emit('add-tweet',data.data.tweet);
                        globalThis.userName = "";
                        globalThis.tweetMessage = "";
                    }});
            }

        },
        data: function () {
            return {
                tweetMessage: '',
                userName: '',
            }
        },
    }
</script>

<style scoped>
    .tweet-form-container {
        flex: 1;
    }

    .row-input {
        display: flex;
        flex-direction: row;
        margin: auto;
    }

    .form-control {
        margin-left: 1%;
        margin-right: 1%;
    }

    .form-control textarea {
        vertical-align: top;
    }

    .row-button {
        float: right;
    }

    .tweet-button {
        background-color: dodgerblue;
        border: none;
        color: aliceblue;
        padding: 10px 20px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        margin: 4px 2px;
        cursor: pointer;
        border-radius: 16px;
        font-weight: bold;
    }

    label {
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        font-weight: bold;
    }

    .input-style {
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        width: 100%;
    }

    .input-inline {
        height: 30px;
        font-size: 14pt;
    }
</style>
