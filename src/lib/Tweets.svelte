<script lang="ts">
    let tweet: string;
    let posts = [
        {tweet: 'Here is another post', timestamp: formateTimestamp('01 Oct 2020 10:00:00 GMT'), likes: 0},
        {tweet: 'This is my first post', timestamp: formateTimestamp('31 Dec 2020 08:30:00 GMT'), likes: 0}
    ];

    function submit(): void {
        if (!tweet) { return; }
        // posts.unshift({tweet: tweet, timestamp: formateTimestamp(new Date())}); // gotcha!
        posts = [{tweet: tweet, timestamp: formateTimestamp(new Date()), likes: 0}, ...posts];
        tweet = undefined;
    }

    function formateTimestamp(d: Date|string): string {
        return new Date(d).toLocaleString();
    }
</script>

<template>
    <div id="tweets">
        <h1>Svelte Tweets</h1>

        <!-- Form -->
        <section id="form">
            <textarea bind:value={tweet} cols="30" rows="10" placeholder="Write your tweet..."></textarea>
            <button on:click="{submit}">Tweet It!</button>
        </section>

        <!-- Posts (list) -->
        <ul id="posts">
            {#each posts as {tweet, timestamp, likes}, i}
                <li on:click="{() => {likes++}}">
                    <p>{tweet}</p>
                    <small>{timestamp}</small>
                    <strong>{likes} Likes</strong>
                </li>
            {/each}
        </ul>

        <!-- Debug -->
        <!-- <hr><pre>tweet: {JSON.stringify(tweet)}</pre><pre>posts: {JSON.stringify(posts)}</pre> -->
    </div>
</template>

<style lang="scss">
    #tweets {
        // pre {background: black; color: white; padding: 20px; margin-top: 20px;}

        textarea {display: block; width: 100%;}
        textarea+button {margin-top: 10px;}
        textarea {box-sizing: border-box; padding: 20px;}
        button {padding: 10px 20px;}

        #form+#posts {margin-top: 20px;}
        #form, #posts {background: #CCC; padding: 20px;}
        #posts {
            li {
                list-style: none; cursor: pointer;
                display: flex; justify-content: space-between; align-items: center;
                p+small, small+strong {margin-left: 20px;}
                p {flex: 1;}
                small {flex: 0 0 auto; opacity: 0.4;}
                strong {flex: 0 0 auto;}
            }
            li+li {border-top: 1px dotted black; margin-top: 30px; padding-top: 20px;}
        }
    }
</style>

