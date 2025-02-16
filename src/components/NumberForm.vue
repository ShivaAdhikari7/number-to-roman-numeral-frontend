<template>
    <div class="wrapper mt-4">
        <form class="d-flex flex-column p-3 gap-3" @submit.prevent="fetchRoman">
            <label class="number-label">Enter number to convert to roman</label>
            <input v-model="number" type="number" placeholder="Enter number" class="number-input-field"
                @input="numberChange" />
            <div class="d-flex align-items-center">
                <input type="submit" value="Fetch Roman" class="btn btn-primary btn-submit" />
            </div>
        </form>
        <div v-if="loading">Loading...</div>
        <div v-if="!loading && romanNumeral" class="roman-numeral-container">Roman Numeral for {{ number }} is
            <strong>{{ romanNumeral
                }}</strong>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            number: '',
            romanNumeral: '',
            loading: false,
        }
    },
    methods: {
        async fetchRoman() {

            const endPointUrl = "https://number-to-roman-numeral-backend.onrender.com/"

            try {
                if (!this.number) {
                    alert("Please enter a valid number");
                    return;
                }

                this.loading = true;
                const num = Number(this.number);
                const response = await fetch(`${endPointUrl}/convert-to-roman?number=${num}`);
                const data = await response.json();

                this.romanNumeral = data.romanNumeral;
                this.number = "";
            } catch (err) {
                console.error(err);
            } finally {
                this.loading = false;
            }
        },
        numberChange() {
            this.romanNumeral = "";
        }
    }
}
</script>

<style scoped>
.wrapper {
    max-width: 62.5rem;
    margin: auto;
}

.number-label {
    font-size: 1.25rem;
    font-weight: 500;
}

.number-input-field {
    padding: 0.5rem;
    border-radius: 0.25rem;
    border: 1px solid #adb5bd;
    font-family: inherit;
}

.btn-submit {
    margin-left: auto;
}
</style>
