<template>
    <!--Denne side viser når man klikker ind på den enkelte drink. Og hvordan det bliver displayet-->
    <div class="container text-center">
        <div class="row">
            <div class="col-md-12">
                <h1>{{ drink.strDrink }}</h1>
            </div>
        </div>
        <div class="row">
            <div class="col-md-12">
                <img :src="drink.strDrinkThumb" alt="" style="max-width: 30%; height: auto;">
            </div>
            <div class="row">
                <div class="col-md-12">
                    <div class="row">
                        <div class="col-md-12">
                            <h3 class="alHeader">{{drink.strAlcoholic}}</h3>
                        </div>
                    </div>
                </div>
                <div class="col-md-6 text">
                    <h2>Instructions</h2>
                    <p class="instructions">{{ drink.strInstructions }}</p>
                </div>
                <div class="col-md-6">
                    <h2>Ingredients:</h2>
                    <ul id="ingredientsList" class="ingredients-list list-unstyled">
                        <li v-for="ingredient in ingredients">
                            <span>{{ ingredient }}</span> - <span>{{ measures[ingredients.indexOf(ingredient)] }}</span>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="mt-2">
                <button type="button" class="btn btn-primary"
                    @click="chooseDrink(this.drink.id)">Lav drink</button>
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
export default {
    //data bliver sat til at være tomme arrays og null
    name: 'DrinkItem',
    props: {
        drink: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            ingredients: [],
            measures: [],

        }
    },
    created() {
        //created hook bliver brugt til at kalde metoderne
        this.ingredientsToList()
        this.measuresToList()
    },
    methods: {
        ingredientsToList() {
            // this.ingredients = []
            for (let i = 1; i <= 15; i++) {
                const ingredient = this.drink["strIngredient" + i]
                if (ingredient) {
                    this.ingredients.push(ingredient)
                }
            }
            console.log(this.ingredients)
        },
        measuresToList() {
            // this.ingredients = []
            for (let i = 1; i <= 15; i++) {
                const measure = this.drink["strMeasure" + i]
                if (measure) {
                    this.measures.push(measure)
                }
            }
            console.log(this.measures)
        },
        async chooseDrink(id){
            try {
                    let responsePost = await axios.post('https://drinksmaskinerest.azurewebsites.net/api/Opskrift/' + id)
                    // console.log(response.status + " " + response.statusText)
                    if (responsePost.status == 200) {
                        alert("Drink ready")
                    }
                    if (responsePost.status == 400) {
                        alert("Drink not ready")
                    }
                } catch (exPost) {
                    console.log(exPost.message)
                    alert("Drink not created. \nException: "+exPost.message)
                }

        },
    }
}
</script>