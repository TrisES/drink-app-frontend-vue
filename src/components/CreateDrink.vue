<template>
    <nav class="navbar bg-body-tertiary">
        <div class="container-fluid">
            <span class="navbar-brand mb-0 h1">Create a Drink</span>
        </div>
    </nav>
    <div class="container mt-5 w-75">
        <div class="row mb-2">
            <h3>Drink details</h3>
        </div>

        <!-- Name -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="CreateName" class="form-label">Name:</label>
                <input type="text" class="form-control" id="CreateName" placeholder="fx FireBall" required>
            </div>
        </div>

        <!-- Cocktail Kategori -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="CreateType custom-select">Type:</label>
                <select class="form-control mt-2" id="CreateType" required>
                    <option>None</option>
                    <option>Cocktail</option>
                    <option>Shot</option>
                    <option>Partydrink</option>
                    <option>Beer</option>
                    <option>Coffee/tea</option>
                </select>
                <div class="invalid-feedback">Choose a type</div>
            </div>
        </div>

        <!-- Ingredients -->
        <div class="row" id="CreateIngredients">
            <div class="col mb-3 form-group">
                <label for="CreateInName" class="form-label">Ingredients:</label>
                <input type="text" class="form-control mb-2" id="CreateInName" placeholder="Cola" v-for="n in Count"
                    required>
            </div>
            <div class="col mb-3 form-group">
                <label for="CreateAmount" class="form-label">Amount in ml:</label>
                <input type="number" class="form-control mb-2" id="CreateAmount" placeholder="ml" v-for="n in Count"
                    required>
            </div>
            <!-- <div div class="col mb-3 mt-2 form-group">
                <label for="Unit">Unit</label>
                <select class="form-control custom-select mb-2" id="Unit" v-for="n in Count" required>
                    <option>ML</option>
                    <option>Part</option>
                    <option>Shot</option>
                    <option>OZ</option>
                </select>
                <div class="invalid-feedback">Choose a type</div>
            </div> -->
            <div class="row">
                <div class="textcenter">
                    <button type="button" class="btn btn-primary" @click="count()" v-if="Count < 6">+</button>
                    <button type="button" class="btn btn-danger" @click="countDown()" v-if="Count > 2">-</button>
                    <!-- <button type="button" class="btn btn-danger disabled" v-if="Count=2">-</button> -->
                </div>
            </div>
        </div>

        <!-- Alcoholic ? yes or no -->
        <div class="row mt-3">
            <label for="Alcoholic">Alcoholic: <span id="AlcoholicV">{{ Alcoholic }}</span></label>
        </div>
        <div class="mt-1 mb-4">
            <button type="button" class="btn btn-primary w-50" @click="setAlcoholic(true)">Yes</button>
            <button type="button" class="btn btn-danger w-50" @click="setAlcoholic(false)">No</button>
        </div>

        <!-- Instructions Text Box -->
        <div class="mb-3">
            <label for="intructionsTextArea" class="form-label">Instructions:</label>
            <textarea class="form-control" id="intructionsTextArea" rows="3" placeholder="Write instructions here..."></textarea>
        </div>


        <!-- Image Upload -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="ImageLink" class="form-label">Image link:</label>
                <input type="text" class="form-control" id="ImageLink" placeholder="https://images.com/drinks/mojito.png" required>
            </div>
        </div>

        <!-- Name of Recipe Creator -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="RecipeCreatorName" class="form-label">Recipe Creator Name:</label>
                <input type="text" class="form-control" id="RecipeCreatorName" placeholder="Full Name" required>
            </div>
        </div>

        <!-- TODO: evt. glass type -->

        <!-- Create button / Submit -->
        <div class="text-center">
            <button type="button" class="btn btn-success w-50">Create Drink</button>
        </div>


    </div>
</template>

<script>
export default {
    name: 'CreateDrink',
    data() {
        return {
            Count: 2,
            Alcoholic: "Alcoholic",
        }
    },
    methods: {
        count() {
            if (this.Count < 6)
                this.Count++
        },
        countDown() {
            if (this.Count > 2)
                this.Count--
        },
        setAlcoholic(bool) {
            if (bool)
                this.Alcoholic = "Alcoholic"
            else
                this.Alcoholic = "Non Alcoholic"
        }
    }
}

</script>