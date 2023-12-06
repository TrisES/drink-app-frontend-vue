<template>
    <nav class="navbar">
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
                <input type="text" v-model="DrinkModel.strDrink" class="form-control" id="CreateName" placeholder="fx FireBall" required>
            </div>
        </div>

        <!-- Cocktail Kategori -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="CreateType custom-select">Type:</label>
                <select class="form-control mt-2" v-model="DrinkModel.strCategory" id="CreateType" required>
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
                <input type="text" v-model="DrinkModel.strIngredient1" class="form-control mb-2" id="CreateInName" placeholder="Ingredient1" required>
                <input type="text" v-model="DrinkModel.strIngredient2" class="form-control mb-2" id="CreateInName" placeholder="Ingredient2" required>
                <input type="text" v-model="DrinkModel.strIngredient3" class="form-control mb-2" id="CreateInName" placeholder="Ingredient3" required>
                <input type="text" v-model="DrinkModel.strIngredient4" class="form-control mb-2" id="CreateInName" placeholder="Ingredient4" required>
                <input type="text" v-model="DrinkModel.strIngredient5" class="form-control mb-2" id="CreateInName" placeholder="Ingredient5" required>
                <input type="text" v-model="DrinkModel.strIngredient6" class="form-control mb-2" id="CreateInName" placeholder="Ingredient6" required>
            </div>
            <div class="col mb-3 form-group">
                <label for="CreateAmount" class="form-label">Amount in ml:</label>
                <input type="number" v-model="DrinkModel.strMeasure1" class="form-control mb-2" id="CreateAmount" placeholder="ml" required>
                <input type="number" v-model="DrinkModel.strMeasure2" class="form-control mb-2" id="CreateAmount" placeholder="ml" required>
                <input type="number" v-model="DrinkModel.strMeasure3" class="form-control mb-2" id="CreateAmount" placeholder="ml" required>
                <input type="number" v-model="DrinkModel.strMeasure4" class="form-control mb-2" id="CreateAmount" placeholder="ml" required>
                <input type="number" v-model="DrinkModel.strMeasure5" class="form-control mb-2" id="CreateAmount" placeholder="ml" required>
                <input type="number" v-model="DrinkModel.strMeasure6" class="form-control mb-2" id="CreateAmount" placeholder="ml" required>
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
            <textarea class="form-control" v-model="DrinkModel.strInstructions"  id="intructionsTextArea" rows="3" placeholder="Write instructions here..."></textarea>
        </div>


        <!-- Image Upload -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="ImageLink" class="form-label">Image link:</label>
                <input type="text" class="form-control" v-model="DrinkModel.strDrinkThumb" id="ImageLink" placeholder="https://images.com/drinks/mojito.png" required>
            </div>
        </div>

        <!-- Name of Recipe Creator -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="RecipeCreatorName" class="form-label">Recipe Creator Name:</label>
                <input type="text" class="form-control" v-model="DrinkModel.creator" id="RecipeCreatorName" placeholder="Full Name" required>
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
            Alcoholic: "Alcoholic",
            DrinkModel: {
                "idDrink": null,
                "strDrink": "",
                "strTags": null,
                "strVideo": null,
                "strCategory": "",
                "strIBA": null,
                "strAlcoholic": "",
                "strGlass": null,
                "strInstructions": "",
                "strDrinkThumb": "",
                "strIngredient1": "",
                "strIngredient2": "",
                "strIngredient3": null,
                "strIngredient4": null,
                "strIngredient5": null,
                "strIngredient6": null,
                "strMeasure1": "",
                "strMeasure2": "",
                "strMeasure3": null,
                "strMeasure4": null,
                "strMeasure5": null,
                "strMeasure6": null,
                "strImageSource": null,
                "id": 0,
                "creator": "string"
            }
        }
    },
    methods: {
        setAlcoholic(bool) {
            if (bool) {
                this.Alcoholic = "Alcoholic"
                this.DrinkModel.strAlcoholic = 'Alcoholic'
            }
            else{
                this.Alcoholic = "Non Alcoholic"
                this.DrinkModel.strAlcoholic = 'Non Alcoholic'
            }
        }
    }
}

</script>