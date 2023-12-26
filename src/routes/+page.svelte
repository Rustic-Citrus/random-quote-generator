<script>
import { fade } from "svelte/transition";

const quotes = [
    {
        quote: "Be yourself; everyone else is already taken.",
        author: "Oscar Wilde"
    },
    {
        quote: "I'm selfish, impatient and a little insecure. I make mistakes, I am out of control and at times hard to handle. But if you can't handle me at my worst, then you sure as hell don't deserve me at my best.",
        author: "Marilyn Monroe"
    },
    {
        quote: "Two things are infinite: the universe and human stupidity; and I'm not sure about the universe.",
        author: "Albert Einstein" 
    },
    {
        quote: "So many books, so little time.",
        author: "Frank Zappa"
    },
    {
        quote: "A room without books is like a body without a soul.",
        author: "Marcus Tullius Cicero"
    }
]
let i = 0;
let currentQuote = [quotes[i]];
const incrementIndex = () => {
    i === quotes.length - 1 ? i = 0 : i += 1;
    currentQuote = [];
    setTimeout(() => {
        currentQuote = [quotes[i]];
    }, 400);
};
</script>

<div class="frame">
    <div class="outer">
        <div class="innerTop">
            {#each currentQuote as quote (quote.quote)}
                <blockquote out:fade={{ duration: 400 }} in:fade={{ duration: 1000 }}>
                    <p>{quote.quote}</p>
                    <footer>{quote.author}</footer>
                </blockquote>
            {/each}
        </div>
        <div class="innerBottom">
            <button id="generateNewQuoteButton" on:click={incrementIndex}>Generate New Quote</button>
        </div>
    </div>
</div>

<style>
@import url("https://fonts.googleapis.com/css2?family=Dancing+Script&display=swap");

.frame {
    display: flex;
    justify-content: center;
    align-items: center;
    background: radial-gradient(
        circle,
        #fbf6ef 0%,
        #dce0d9 35%,
        #ead7c3  100%
    );
    width: 100vw;
    height: 100vh;
}

.outer {
    width: 67%;
    height: 33%;
    display: flex;
    flex-direction: column;
    border-radius: 10px;
    background: radial-gradient(
        circle,
        #fbf6ef 0%,
        #fbf6ef 80%,
        #FFFFFF  100%
    );
    box-shadow: 0px 1px 4px 0px #888;
}

.innerTop {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 75%;
}

.innerBottom {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 25%;
}

blockquote {
    text-align: center;
    transform: opacity 0.5s ease-in-out;
}

blockquote p {
    font-family: 'Dancing Script', cursive;
    font-size: 1.4rem;
}

blockquote footer {
    font-family: 'Courier New', Courier, monospace;
    font-size: 1rem;
}

button {
    width: 33%;
    height: 2rem;
    border: solid 1px #888;
    border-radius: 4px;
    box-shadow: 0px 1px 4px 0px #888;
}
</style>