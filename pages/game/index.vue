<template>
    <main>
        <h1>The Guesser Game</h1>
        <div>
            <section>
                <p class="text-neutral-500 mt-2">Deck ID: {{ deckId }}</p>

                <div v-if="deckId" class="mt-8 flex flex-row gap-x-4">
                    <Button color="danger" type="button" @click="setGuess('red')">Pick
                        Red</Button>
                    <Button color="dark" type="button" @click="setGuess('black')">Pick
                        Black</Button>
                </div>

                <p v-if="guess" class="mt-4">Current Pick: <strong>{{ guess }}</strong></p>

                <!-- Show the score -->
                <h2 v-if="deckId" class="mt-6">{{ score }} Points</h2>

                <Button class="mt-6" color="primary" v-if="guess" @click="drawCard()">Draw Card</Button>
            </section>
            <section class="mt-8">
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