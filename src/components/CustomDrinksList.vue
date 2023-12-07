<template>
    <NavBar v-if="currentPage == 'CustomDrinksList'" />
    <div class="text-center">
        <button v-if="currentPage == 'liste' || 'CustomDrinkList' " type="button" class="btn btn-success w-50" @click="setCurrentPage('CreateDrink')">Create Drink</button>
    </div>
    <CreateDrink v-if="currentPage == 'CreateDrink'"></CreateDrink>
    <div v-if="currentPage == 'liste'" class="mt-1 mt-5" style="text-align: center;">
        <button type="button" class="btn border-black" id="sortByAsc" v-on:click="sortByDrinksName()">↑</button>
        <button type="button" class="btn border-black" id="sortByDsc" v-on:click="sortByDrinksNameR()">↓</button>
    </div>


    <ul id="drinksListe" v-if="currentPage == 'liste'" class="list-group" style="text-align: center;">
        <li class="list-Group-item mt-2" v-for="drink in drinks">
            <div class="row">
                <div class="col-3">
                </div>
                <div class="col-3 border-bottom" style="text-align: center;">
                    <div class="media">
                        <div class="media-left">
                            <div class="media-body">
                                <div class="drinkNavn">
                                    <h4 class="media-heading">{{ drink.strDrink }}</h4>
                                </div>
                                Ingredienter: <span class="Indregient1">{{ drink.strIngredient1 }}</span> og <span
                                    class="Indregient2">{{ drink.strIngredient2 }}</span><br>
                                <span class="AlkoholJaNej"> <b>{{ drink.strAlcoholic }}</b> </span>
                            </div>
                            <div class="mt-2">
                                <button type="button" class="btn border-black readButton" @click="setDrink(drink) + setCurrentPage('DrinkItem');" >Læs
                                    mere</button>
                                <!-- referer til drinkItem, og hide/unhide alt efter læs mere knap -->

                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-3 border-bottom">
                    <div class="media">
                        <div class="media-left">
                            <a href="#">
                                <img class="media-object mt-2 mb-2" id="listeBillede" :src="drink.strDrinkThumb" alt="..."
                                    style="height: 100px; width: 100px;">
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </li>
    </ul>
    <button v-if="currentPage == 'DrinkItem'" type="button" class="btn border-black readButton" @click="setCurrentPage('liste') + setDrinkNull()">tilbage</button>
    <DrinkItem v-if="currentPage == 'DrinkItem'" :drink="currentDrink" />
</template>

<script>
import axios from 'axios';
import DrinkItem from './DrinkItem.vue';
import CreateDrink from './CreateDrink.vue'
import NavBar from './NavBar.vue';
const baseUrl = "https://www.thecocktaildb.com/api/json/v1/1/search.php?s="
export default {
    name: 'DrinkList',
    data() {
        return {
            alldrinks: [],
            drinks: [],
            drink: null,
            ingredients: [],
            ingredient: null,
            currentDrink: null,
            searchToGetBy: "",
            searchToGetByForIngredient: "",
            currentPage: "liste"
        };
    },
    async created() {
        try {
            const response = await axios.get(baseUrl);
            this.drinks = await response.data.drinks;
            this.sortByDrinksName(this.drinks);
            console.log(this.drinks);
            this.alldrinks = this.drinks;
        }
        catch (ex) {
            alert(ex.message);
        }
    },
    methods: {
        setDrink(drink) {
            this.currentDrink = drink;
        },
        setCurrentPage(page){
            this.currentPage = page
        },
        setDrinkNull() {
            this.currentDrink = null;
        },
        getAlldrinks() {
            this.drinks = this.alldrinks;
        },
        sortByDrinksName() {
            this.drinks.sort((drink1, drink2) => drink1.strDrink.localeCompare(drink2.strDrink));
        },
        sortByDrinksNameR() {
            this.drinks.sort((drink1, drink2) => drink2.strDrink.localeCompare(drink1.strDrink));
        },
        filterShots(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Shot"));
        },
        filterOrdinaryDrink(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Ordinary Drink"));
        },
        filterCocktails(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Cocktail"));
        },
        filterBeer() {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Beer"));
        },
        filterCoffeeTea(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Coffee / Tea"));
        },
        filterPunchParty(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Punch / Party Drink"));
        },
        filterAlcholic(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strAlcoholic.includes("Alcoholic"));
        },
        filterNonAlcholic(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strAlcoholic.includes("Non alcoholic"));
        },
        getByName(search) {
            const url = baseUrl + search
            console.log(url)
            this.helperGetAndShow(url)
        },
        getByIngredient(search) {
            if (search != null) {
                this.drinks = this.alldrinks.filter(b => b.strIngredient1.includes(search) || b.strIngredient2.includes(search))
                return this.drinks
            }
            else {
                return "No drinks found"
            }
        },
        async helperGetAndShow(url) {
            try {
                const response = await axios.get(url)
                this.drinks = await response.data.drinks
            } catch (ex) {
                alert(ex.message)
            }
        },
    },
    components: { DrinkItem, CreateDrink }
    

}
</script>