<template>
    <div class="tweets-table-container">
        <div class="tweet-header">
            <span @click="setOrderKey('name')" class="tweet-header-cell tweet-name">
                Name
                <i v-if="orderKey === 'name' && orderDesc" class="icon sort-icon sort-icon-desc" />
                <i v-if="orderKey === 'name' && !orderDesc" class="icon sort-icon sort-icon-asc" />
            </span>
            <span class="tweet-header-cell tweet-message">Message</span>
            <span @click="setOrderKey('datetime')" class="tweet-header-cell tweet-moment">
                Datetime
                <i v-if="orderKey === 'datetime' && orderDesc" class="icon sort-icon sort-icon-desc" />
                <i v-if="orderKey === 'datetime' && !orderDesc" class="icon sort-icon sort-icon-asc" />
            </span>
        </div>
        <div class="tweet-row" v-bind:key="t.id" v-for="t in orderedTweets">
            <span class="tweet-cell tweet-name">{{t.name}}</span>
            <span class="tweet-cell tweet-message">{{t.message}}</span>
            <span class="tweet-cell tweet-moment">{{t.datetime}}</span>
        </div>
    </div>
</template>

<script>
    const compareTweets = (tweetA, tweetB, key, desc = false) => {
        if (desc) {
            if (tweetA[key] > tweetB[key]) return -1;
            if (tweetA[key] < tweetB[key]) return 1;
        } else {
            if (tweetA[key] > tweetB[key]) return 1;
            if (tweetA[key] < tweetB[key]) return -1;
        }

        return 0;
    };
    export default {
        name: 'TweetsTable',
        methods: {
            orderTweets: function(tweets, key, desc) {
                return tweets.sort((tA, tB) => compareTweets(tA, tB, key, desc));
            },
            setOrderKey: function (key) {
                if (key === this.orderKey) {
                    this.setOrderDesc(!this.orderDesc);
                    return;
                }
                this.orderKey = key;
                this.orderDesc = key === 'datetime';
            },
            setOrderDesc: function (desc) {
                this.orderDesc = !!desc;
            },
        },
        props: ['tweets'],
        data: function() {
            return {
                orderKey: 'datetime',
                orderDesc: true,
            }
        },
        computed: {
            orderedTweets: function() {
                return this.orderTweets(this.tweets, this.orderKey, this.orderDesc);
            }
        }
    }
</script>

<style scoped>
    .tweets-table-container {
        flex: 1;
    }

    .tweet-header-cell {
        padding: 5px 15px;
        display: inline-block;
        font-weight: 700;
        cursor: pointer;
    }

    .icon {
        display: inline-block;
        vertical-align: middle;
        background: transparent center center no-repeat;
    }

    .icon.sort-icon {
        width: 15px;
        height: 7px;
    }

    .icon.sort-icon.sort-icon-asc {
        background-image: url("../assets/arrow-up.png");
    }
    .icon.sort-icon.sort-icon-desc {
        background-image: url("../assets/arrow-bottom.png");
    }

    .tweet-row {
        border-radius: 5px;
    }

    .tweet-row:nth-child(2n + 1) {
        background: rgba(80, 80, 220, 0.1);
    }

    .tweet-cell {
        padding: 5px 15px;
        display: inline-block;
        overflow: auto;
    }

    .tweet-id {
        width: 30px;
    }

    .tweet-name {
        width: 100px;
    }

    .tweet-message {
        width: 300px;
    }
</style>
