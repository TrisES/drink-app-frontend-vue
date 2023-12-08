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
                <label for="CreateName" class="form-label fw-bold">Name:</label>
                <input type="text" v-model="DrinkModel.strDrink" class="form-control" id="CreateName"
                    placeholder="fx FireBall" required>
            </div>
        </div>

        <!-- Cocktail Kategori -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="CreateType custom-select" class="fw-bold">Type:</label>
                <select class="form-control mt-2" aria-label="Small select example" v-model="DrinkModel.strCategory"
                    id="CreateType" required>
                    <!-- <option selected>Choose a Category</option> -->
                    <option value="Cocktail" selected>Cocktail</option>
                    <option value="Shot">Shot</option>
                    <option value="Partydrink">Partydrink</option>
                    <option value="Beer">Beer</option>
                    <option value="Coffee / Tea">Coffee / Tea</option>
                </select>
                <div class="invalid-feedback">Choose a type</div>
            </div>
        </div>

        <!-- Ingredients -->
        <div class="row" id="CreateIngredients">
            <div class="col mb-3 form-group">
                <label for="CreateInName" class="form-label fw-bold">Ingredients:</label>
                <input type="text" v-model="DrinkModel.strIngredient1" class="form-control mb-2" placeholder="Ingredient1"
                    required>
                <input type="text" v-model="DrinkModel.strIngredient2" class="form-control mb-2" placeholder="Ingredient2"
                    required>
                <input type="text" v-model="DrinkModel.strIngredient3" class="form-control mb-2" placeholder="Ingredient3">
                <input type="text" v-model="DrinkModel.strIngredient4" class="form-control mb-2" placeholder="Ingredient4">
                <input type="text" v-model="DrinkModel.strIngredient5" class="form-control mb-2" placeholder="Ingredient5">
                <input type="text" v-model="DrinkModel.strIngredient6" class="form-control mb-2" placeholder="Ingredient6">
            </div>
            <div class="col mb-3 form-group">
                <label for="CreateAmount" class="form-label fw-bold">Amount in ml:</label>
                <input type="number" v-model="DrinkModel.strMeasure1" class="form-control mb-2" placeholder="ml" required>
                <input type="number" v-model="DrinkModel.strMeasure2" class="form-control mb-2" placeholder="ml" required>
                <input type="number" v-model="DrinkModel.strMeasure3" class="form-control mb-2" placeholder="ml">
                <input type="number" v-model="DrinkModel.strMeasure4" class="form-control mb-2" placeholder="ml">
                <input type="number" v-model="DrinkModel.strMeasure5" class="form-control mb-2" placeholder="ml">
                <input type="number" v-model="DrinkModel.strMeasure6" class="form-control mb-2" placeholder="ml">
            </div>
        </div>

        <!-- Alcoholic ? yes or no -->
        <div class="row mt-3">
            <label for="Alcoholic" class="fw-bold">Alcoholic: <span id="AlcoholicV">{{ Alcoholic }}</span></label>
        </div>
        <div class="mt-1 mb-4">
            <button type="button" class="btn btn-primary w-50" @click="setAlcoholic(true)">Yes</button>
            <button type="button" class="btn btn-danger w-50" @click="setAlcoholic(false)">No</button>
        </div>

        <!-- Instructions Text Box -->
        <div class="mb-3">
            <label for="intructionsTextArea" class="form-label fw-bold">Instructions:</label>
            <textarea class="form-control" v-model="DrinkModel.strInstructions" id="intructionsTextArea" rows="3"
                placeholder="Write instructions here..."></textarea>
        </div>


        <!-- Image Upload -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="ImageLink" class="form-label fw-bold">Image link:</label>
                <input type="text" class="form-control" v-model="DrinkModel.strDrinkThumb" id="ImageLink"
                    placeholder="https://images.com/drinks/mojito.png" required>
            </div>
        </div>

        <!-- Name of Recipe Creator -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="RecipeCreatorName" class="form-label fw-bold">Recipe Creator Name:</label>
                <input type="text" class="form-control" v-model="DrinkModel.creator" id="RecipeCreatorName"
                    placeholder="Full Name" required>
            </div>
        </div>

        <!-- TODO: evt. glass type -->

        <!-- Create button / Submit -->
        <div class="text-center">
            <button type="button" class="btn btn-success w-50" @click="createDrink()">Create Drink</button>
        </div>


    </div>
</template>

<script>
import axios from 'axios';

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
                "strCategory": "Cocktail",
                "strIBA": null,
                "strAlcoholic": "Alcoholic",
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
                "creator": "Name"
            }
        }
    },
    methods: {
        setAlcoholic(bool) {
            if (bool) {
                this.Alcoholic = "Alcoholic"
                this.DrinkModel.strAlcoholic = 'Alcoholic'
            }
            else {
                this.Alcoholic = "Non Alcoholic"
                this.DrinkModel.strAlcoholic = 'Non Alcoholic'
            }
        },
        async createDrink() {
            this.MeasuresToString()
            if (this.ValidateModel()) {
                try {
                    console.log(this.DrinkModel)
                    let responsePost = await axios.post('https://drinksmaskinerest.azurewebsites.net/api/DrinkModel', this.DrinkModel)
                    // console.log(response.status + " " + response.statusText)
                    if (responsePost.status == 200) {
                        alert("Drink created")
                    }
                    if (responsePost.status == 400) {
                        alert("Drink not created")
                    }
                } catch (exPost) {
                    console.log(exPost.message)
                    alert("Drink not created. \nException: "+exPost.message)
                }
            }

        },
        MeasuresToString() {
            // convert int to string
            for (let i = 1; i < 7; i++) {
                if (this.DrinkModel["strMeasure" + i] != null) {
                    this.DrinkModel["strMeasure" + i] = this.DrinkModel["strMeasure" + i].toString()
                }
            }
        },
        ValidateModel() {
            if (this.DrinkModel.strDrink.trim() == "") {
                alert("Name is required")
                return false;
            }
            else if (this.DrinkModel.strCategory.trim() == "") {
                alert("Category is required")
                return false;
            }
            else if (this.DrinkModel.strIngredient1.trim() == "") {
                alert("Ingredient1 is required")
                return false;
            }
            else if (this.DrinkModel.strMeasure1.trim() == "") {
                alert("Amount of Ingredient1 is required")
                return false;
            }
            else if (this.DrinkModel.strIngredient2.trim() == "") {
                alert("Ingredient2 is required")
                return false;
            }
            else if (this.DrinkModel.strMeasure2.trim() == "") {
                alert("Amount of Ingredient2 is required")
                return false;
            }
            else if (this.DrinkModel.strInstructions.trim() == "") {
                alert("Instructions is required")
                return false;
            }
            // else if (this.DrinkModel.strDrinkThumb == "") {
            //     alert("Image link is required")
            // }
            else if (this.DrinkModel.creator == "") {
                alert("Creator name is required")
                return false;
            }
            return true;
        }
    }
}

</script>