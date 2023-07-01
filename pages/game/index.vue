<template>
    <main>
        <h1>The Guesser Game</h1>
        <div>
            <section>
                <GeneralButton color="primary">New Deck</GeneralButton>
                <!-- Keep track of deck id -->
                <p>Deck ID: {{ deckId }}</p>

                <div v-if="deckId" role="group" aria-label="Color Picker">
                    <Button color="danger" type="button" @click="setGuess('red')">Pick
                        Red</Button>
                    <Button color="dark" type="button" @click="setGuess('black')">Pick
                        Black</Button>
                </div>

                <p v-if="guess">Current Pick: <strong>{{ guess }}</strong></p>

                <!-- Show the score -->
                <h2 v-if="deckId">{{ score }} Points</h2>

                <Button color="primary" v-if="guess" @click="drawCard()">Draw Card</Button>
            </section>
            <section>
                <!-- Show the drawn card -->
                <div v-if="card">
                    <img :src="card.image" :alt="card.code">
                </div>
            </section>
        </div>
    </main>
</template>


<script setup>

const score = ref(0);

const deckId = ref();

onMounted(async () => {
    const { deck_id } = await $fetch(
        'https://www.deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1'
    );
    deckId.value = deck_id;
});

const guess = ref();
const card = ref();

function setGuess(color) {
    guess.value = color;
}

async function drawCard() {
    const API = `https://www.deckofcardsapi.com/api/deck/${deckId.value}/draw/?count=1`;

    const data = await fetch(API).then((res) => res.json());
    card.value = data.cards[0];
    evaluateGuess();
}

function evaluateGuess() {
    // Figure out color of card
    const { suit } = card.value;
    // Check if guess matches the color

    const colormatch = {
        red: ['HEARTS', 'DIAMONDS'],
        black: ['SPADES', 'CLUBS'],
    };

    return colormatch[guess.value].includes(suit) ? score.value++ : false;
    // if match increment the score
}

</script>