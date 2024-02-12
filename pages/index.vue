<template>
    <div>
        <div class="input_field">
        <h1>同時投稿くん</h1>
        <label>
            <p>Bluesky ID ※@は抜いて、bskyから後も入力してください！※</p>
            <input type="text" v-model="bs_id">
            <p>Bluesky パスワード</p>
            <input type="password" v-model="bs_pw">
            <p>投稿する内容</p>
            <textarea name="post_content" cols="25" rows="12" v-model="ct"></textarea>
            <p>Blueskyは自動で投稿するから最終確認してくださいね</p>
            <button @click="btClicked">投稿する</button>
        </label>
        <div class="er_log" id="er_log"></div>
        <p></p><a href="https://twitter.com/tttaaayou">作者のXを見る</a>
        </div>
    </div>
</template>

<script>
import {BskyAgent} from '@atproto/api'

    export default {
        data () {
            return {
                bs_id: '',
                bs_pw: '',
                ct: '',
            }
        },
        methods: {
            async btClicked () {

                if(!this.bs_id || !this.bs_pw || !this.ct) {
                    console.log("入力されていない値があるよ")
                    document.getElementById('er_log').innerHTML = "<p>入力されていない箇所があります</p>";
                }
                else {
                    document.getElementById('er_log').innerHTML = "";

                    console.log("bs_id: " + this.bs_id)
                    console.log("bs_pw: " + this.bs_pw)
                    console.log("content: " + this.ct)

                    const agent = new BskyAgent({ service: "https://bsky.social"});

                    await agent.login({
                        identifier: this.bs_id,
                        password: this.bs_pw,
                    })

                    await agent.post({
                        text: this.ct,
                        langs: ["ja"]
                    })
                    document.getElementById('er_log').innerHTML = "ID、パスでミスがない限り正常に投稿完了";
                    window.open("https://twitter.com/intent/tweet?text=" + encodeURIComponent(this.ct))
                }
            }
        }
    }
</script>

<style>
    html {
        font: 400 16px -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
        -ms-text-size-adjust: 100%;
        -webkit-text-size-adjust: 100%;
        -moz-osx-font-smoothing: grayscale;
        -webkit-font-smoothing: antialiased;
        box-sizing: border-box;
    }

    a {
        margin: 2px;
        padding: 6px;
        color: #4169e1;
        text-decoration: none;
    }

    a:hover {
        color: #f0f8ff;
        background-color: #4169e1;
        border-radius: 4px;
    }

    h1 {
        text-align: center;
    }

    .input_field {
        padding: 10px;
        margin: 0 auto;
        width: 700px;
        height: auto;
        background-color: #f5f5f5;
    }
    input {
        font-size: 20px;
    }
    textarea {
        resize: none;
        font-size: 20px;
    }
    button {
        width: 500px;
        height: 50px;
    }

    .er_log {
        height: 30px;
    }
</style>