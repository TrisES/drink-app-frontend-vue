<template>
    <nav class="navbar">
        <div class="container-fluid">
            <span class="navbar-brand mb-0 h1">Update a Drink</span>
        </div>
    </nav>
    <div class="container mt-5 w-75">
        <div class="row mb-2">
            <h3>Drink to Update</h3>
        </div>

        <!-- Name -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="CreateName" class="form-label fw-bold">Name:</label>
                <input type="text" v-model="DrinkModelNew.strDrink" class="form-control" id="CreateName"
                    placeholder="fx FireBall" required>
            </div>
        </div>

        <!-- Cocktail Kategori -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="CreateType custom-select" class="fw-bold">Type:</label>
                <select class="form-control mt-2" aria-label="Small select example" v-model="DrinkModelNew.strCategory"
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
                <input type="text" v-model="DrinkModelNew.strIngredient1" class="form-control mb-2"
                    placeholder="Ingredient1" required>
                <input type="text" v-model="DrinkModelNew.strIngredient2" class="form-control mb-2"
                    placeholder="Ingredient2" required>
                <input type="text" v-model="DrinkModelNew.strIngredient3" class="form-control mb-2"
                    placeholder="Ingredient3">
                <input type="text" v-model="DrinkModelNew.strIngredient4" class="form-control mb-2"
                    placeholder="Ingredient4">
                <input type="text" v-model="DrinkModelNew.strIngredient5" class="form-control mb-2"
                    placeholder="Ingredient5">
                <input type="text" v-model="DrinkModelNew.strIngredient6" class="form-control mb-2"
                    placeholder="Ingredient6">
            </div>
            <div class="col mb-3 form-group">
                <label for="CreateAmount" class="form-label fw-bold">Amount in ml:</label>
                <input type="number" v-model="DrinkModelNew.strMeasure1" class="form-control mb-2" placeholder="ml"
                    required>
                <input type="number" v-model="DrinkModelNew.strMeasure2" class="form-control mb-2" placeholder="ml"
                    required>
                <input type="number" v-model="DrinkModelNew.strMeasure3" class="form-control mb-2" placeholder="ml">
                <input type="number" v-model="DrinkModelNew.strMeasure4" class="form-control mb-2" placeholder="ml">
                <input type="number" v-model="DrinkModelNew.strMeasure5" class="form-control mb-2" placeholder="ml">
                <input type="number" v-model="DrinkModelNew.strMeasure6" class="form-control mb-2" placeholder="ml">
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
            <textarea class="form-control" v-model="DrinkModelNew.strInstructions" id="intructionsTextArea" rows="3"
                placeholder="Write instructions here..."></textarea>
        </div>


        <!-- Image Upload -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="ImageLink" class="form-label fw-bold">Image link:</label>
                <input type="text" class="form-control" v-model="DrinkModelNew.strDrinkThumb" id="ImageLink"
                    placeholder="https://images.com/drinks/mojito.png" required>
            </div>
        </div>

        <!-- Name of Recipe Creator -->
        <div class="row">
            <div class="col mb-3 form-group">
                <label for="RecipeCreatorName" class="form-label fw-bold">Recipe Creator Name:</label>
                <input type="text" class="form-control" v-model="DrinkModelNew.creator" id="RecipeCreatorName"
                    placeholder="Full Name" required>
            </div>
        </div>

        <!-- TODO: evt. glass type -->

        <!-- Create button / Submit -->
        <div class="text-center">
            <button type="button" class="btn btn-success w-50" @click="updateDrink()">Update</button>
        </div>


    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'UpdateDrink',
    props: {
        idToUpdate: {
            type: Number,
            required: true
        },
        drinkToUpdate: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            Alcoholic: this.drinkToUpdate.strAlcoholic,
            DrinkModelNew: {
                "idDrink": this.drinkToUpdate.idDrink || '',
                "strDrink": this.drinkToUpdate.strDrink || '',
                "strTags": null || '',
                "strVideo": null || '',
                "strCategory": this.drinkToUpdate.strCategory || '',
                "strIBA": null || '',
                "strAlcoholic": this.drinkToUpdate.strAlcoholic || '',
                "strGlass": null || '',
                "strInstructions": this.drinkToUpdate.strInstructions || '',
                "strDrinkThumb": this.drinkToUpdate.strDrinkThumb || '',
                "strIngredient1": this.drinkToUpdate.strIngredient1 || '',
                "strIngredient2": this.drinkToUpdate.strIngredient2 || '',
                "strIngredient3": this.drinkToUpdate.strIngredient3 || '',
                "strIngredient4": this.drinkToUpdate.strIngredient4 || '',
                "strIngredient5": this.drinkToUpdate.strIngredient5 || '',
                "strIngredient6": this.drinkToUpdate.strIngredient6 || '',
                "strMeasure1": this.drinkToUpdate.strMeasure1 || '',
                "strMeasure2": this.drinkToUpdate.strMeasure2 || '',
                "strMeasure3": this.drinkToUpdate.strMeasure3 || '',
                "strMeasure4": this.drinkToUpdate.strMeasure4 || '',
                "strMeasure5": this.drinkToUpdate.strMeasure5 || '',
                "strMeasure6": this.drinkToUpdate.strMeasure6 || '',
                "strImageSource": this.drinkToUpdate.strImageSource || '',
                "id": this.idToUpdate || '',
                "creator": this.drinkToUpdate.creator || ''
            }
        }
    },
    methods: {
        setAlcoholic(bool) {
            if (bool) {
                this.Alcoholic = "Alcoholic"
                this.DrinkModelNew.strAlcoholic = 'Alcoholic'
            }
            else {
                this.Alcoholic = "Non Alcoholic"
                this.DrinkModelNew.strAlcoholic = 'Non Alcoholic'
            }
        },
        async updateDrink() {
            this.MeasuresToString()
            if (this.ValidateModel()) {
                try {
                    console.log(this.DrinkModelNew)
                    let responsePut = await axios.put('https://drinksmaskinerest.azurewebsites.net/api/DrinkModel/' + this.idToUpdate, this.DrinkModelNew)
                    // console.log(response.status + " " + response.statusText)
                    if (responsePut.status == 200) {
                        alert("Drink Updated")
                    }
                    if (responsePut.status == 400) {
                        alert("Drink not Updated")
                    }
                } catch (exPut) {
                    console.log(exPut.message)
                    alert("Drink not created. \nException: " + exPut.message)
                }
            }

        },
        MeasuresToString() {
            // convert int to string
            for (let i = 1; i < 7; i++) {
                if (this.DrinkModelNew["strMeasure" + i] != null) {
                    this.DrinkModelNew["strMeasure" + i] = this.DrinkModelNew["strMeasure" + i].toString()
                }
            }
        },
        ValidateModel() {
            if (this.DrinkModelNew.strDrink.trim() == "") {
                alert("Name is required")
                return false;
            }
            else if (this.DrinkModelNew.strCategory.trim() == "") {
                alert("Category is required")
                return false;
            }
            else if (this.DrinkModelNew.strIngredient1.trim() == "") {
                alert("Ingredient1 is required")
                return false;
            }
            else if (this.DrinkModelNew.strMeasure1.trim() == "") {
                alert("Amount of Ingredient1 is required")
                return false;
            }
            else if (this.DrinkModelNew.strIngredient2.trim() == "") {
                alert("Ingredient2 is required")
                return false;
            }
            else if (this.DrinkModelNew.strMeasure2.trim() == "") {
                alert("Amount of Ingredient2 is required")
                return false;
            }
            else if (this.DrinkModelNew.strInstructions.trim() == "") {
                alert("Instructions is required")
                return false;
            }
            // else if (this.DrinkModelNew.strDrinkThumb == "") {
            //     alert("Image link is required")
            // }
            else if (this.DrinkModelNew.creator == "") {
                alert("Creator name is required")
                return false;
            }
            return true;
        }
    }
}

</script>