<script>
    // import { fade } from "svelte/transition";
    import { crossfade } from "svelte/transition";
    import { quintOut } from "svelte/easing";
    import data from "../data/quotes.json";

    const [send, receive] = crossfade({
        duration: (d) => Math.sqrt(d * 200),

        fallback(node, params) {
            const style = getComputedStyle(node);
            const transform = style.transform === "none" ? "" : style.transform;
            return {
                duration: 600,
                easing: quintOut,
                css: (t) => `
                transform: ${transform} scale(${t});
                opacity: ${t}
            `,
            };
        },
    });

    const quotes = data;
    quotes.sort(() => Math.random() - 0.5);
    let i = 0;
    let currentQuote = quotes[i];

    const handleNewQuote = () => {
        i++;
        if (i === quotes.length) {
            i = 0;
        }
        currentQuote = quotes[i];
    };

    const handleTweet = () => {
        const tweetQuote = currentQuote.quote.replace(/ /g, "%20");
        const tweetAuthor = currentQuote.author.replace(/ /g, "%20");
        const tweetLink = `https://twitter.com/intent/tweet?text=${tweetQuote} - ${tweetAuthor}`;
        window.open(tweetLink, "_blank");
    };
</script>

<div class="frame">
    <div class="card-outer" id="quote-box">
        <div class="card-inner-top">
            {#each [currentQuote] as quote (quote.quote)}
                <blockquote
                    out:send={{ key: currentQuote.quote }}
                    in:receive={{ key: currentQuote.quote }}
                >
                    <p id="text">{quote.quote}</p>
                    <footer id="author">{quote.author}</footer>
                </blockquote>
            {/each}
        </div>
        <div class="card-inner-bottom">
            <button id="new-quote" on:click={handleNewQuote}
                >Generate New Quote</button
            >
            <button id="tweet-quote" on:click={handleTweet}>
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="16"
                    height="16"
                    fill="black"
                    class="bi bi-twitter-x"
                    viewBox="0 0 16 16"
                >
                    <path
                        d="M12.6.75h2.454l-5.36 6.142L16 15.25h-4.937l-3.867-5.07-4.425 5.07H.316l5.733-6.57L0 .75h5.063l3.495 4.633L12.601.75Zm-.86 13.028h1.36L4.323 2.145H2.865z"
                    />
                </svg>
                Tweet This Quote
            </button>
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
        height: 60%;
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
        flex-direction: row;
        justify-content: center;
        align-items: center;
        height: 25%;
        gap: 0.5rem;
    }

    blockquote {
        text-align: center;
        transform: opacity 0.5s ease-in-out;
        position: absolute;
        width: 50%;
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

    button svg {
        margin-right: 0.5rem;
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

        blockquote {
            width: 80%;
        }

        blockquote p {
            font-size: 1.4rem;
        }

        .card-inner-bottom {
            flex-direction: column;
        }
    }

    @media only screen and (min-width: 320px) and (max-width: 576px) {
        .card-outer {
            width: 80%;
            height: 75%;
        }

        button {
            width: 75%;
            height: 2rem;
            font-size: 1rem;
        }

        blockquote {
            width: 75%;
        }

        .card-inner-bottom {
            flex-direction: column;
        }
    }

    @media only screen and (min-width: 576px) and (max-width: 768px) {
        .card-outer {
            width: 75%;
            height: 67%;
        }

        button {
            width: 50%;
            height: 2.5rem;
            font-size: 0.9rem;
        }

        blockquote {
            width: 65%;
        }

        .card-inner-top {
            height: 60%;
        }

        .card-inner-bottom {
            flex-direction: column;
            height: 40%;
        }
    }
</style>
