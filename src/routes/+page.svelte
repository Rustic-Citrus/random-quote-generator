<script>
    import { fade } from "svelte/transition";
    import data from "../data/quotes.json";

    const quotes = data;
    let i = 0;
    let currentQuote = [quotes[i]];
    let prevIndicies = [i];
    const incrementIndex = () => {
        if (prevIndicies.includes(i) && prevIndicies.length < quotes.length) {
            i = Math.floor(Math.random() * quotes.length);
            prevIndicies.push(i);
        } else if (
            prevIndicies.includes(i) &&
            prevIndicies.length >= quotes.length
        ) {
            prevIndicies = new Array();
            i = Math.floor(Math.random() * quotes.length);
            prevIndicies.push(i);
        } else {
            i = Math.floor(Math.random() * quotes.length);
            prevIndicies.push(i);
        }
        currentQuote = [];
        setTimeout(() => {
            currentQuote = [quotes[i]];
        }, 400);
    };
</script>

<div class="frame">
    <div class="card-outer">
        <div class="card-inner-top">
            {#each currentQuote as quote (quote.quote)}
                <blockquote
                    out:fade={{ duration: 400 }}
                    in:fade={{ duration: 1000 }}
                >
                    <p>{quote.quote}</p>
                    <footer>{quote.author}</footer>
                </blockquote>
            {/each}
        </div>
        <div class="card-inner-bottom">
            <button id="generateNewQuoteButton" on:click={incrementIndex}
                >Generate New Quote</button
            >
        </div>
    </div>
</div>

<style>
    .frame {
        flex: 1 1 auto;
        display: flex;
        justify-content: center;
        align-items: center;
        background: radial-gradient(
            circle,
            #fbf6ef 0%,
            #dce0d9 10%,
            #ead7c3 100%
        );
        margin: 0 auto;
        width: 100vw;
        box-sizing: border-box;
    }

    .card-outer {
        width: 67%;
        height: 40%;
        display: flex;
        flex-direction: column;
        border-radius: 10px;
        background: radial-gradient(
            circle,
            #fbf6ef 0%,
            #fbf6ef 80%,
            #ffffff 100%
        );
        box-shadow: 0px 1px 4px 0px #888;
    }

    .card-inner-top {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 75%;
    }

    .card-inner-bottom {
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
        font-family: "Caveat", cursive;
        font-size: 1.6rem;
    }

    blockquote footer {
        font-family: "Open Sans", sans-serif;
        font-size: 1rem;
    }

    button {
        width: 33%;
        height: 2rem;
        border: solid 1px #888;
        border-radius: 4px;
        box-shadow: 0px 1px 4px 0px #888;
        font-family: "Open Sans", sans-serif;
        font-size: 0.8rem;
        font-weight: bold;
        background-color: #fff;
        transition: background-color 0.8s ease-in-out;
    }

    button:hover {
        cursor: pointer;
        background-color: #ffe9cb;
    }

    @media only screen and (max-width: 320px) {
        .card-outer {
            width: 90%;
            height: 95%;
        }

        button {
            width: 80%;
            height: 3rem;
            font-size: 1rem;
        }

        blockquote p {
            font-size: 1.4rem;
        }
    }

    @media only screen and (min-width: 320px) and (max-width: 576px) {
        .card-outer {
            width: 80%;
            height: 75%;
        }

        button {
            width: 75%;
            height: 3rem;
            font-size: 1rem;
        }
    }

    @media only screen and (min-width: 576px) and (max-width: 768px) {
        .card-outer {
            width: 75%;
            height: 50%;
        }

        button {
            width: 50%;
            height: 2.5rem;
            font-size: 0.9rem;
        }
    }
</style>
