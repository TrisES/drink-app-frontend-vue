<template>
    <div class="mt-1" style="text-align: center;">
        <button type="button" class="btn border-black" id="sortByAsc" v-on:click="sortByDrinksName()">↑</button>
        <button type="button" class="btn border-black" id="sortByDsc" v-on:click="sortByDrinksNameR()">↓</button>
    </div>

    <div class="container mt-1">
        <div class="dropdown" style="text-align: center;">
            <button class="btn btn-dark dropdown-toggle" type="button" id="dropdownMenuButton"
                data-bs-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                Vælg en kategori
            </button>
            <div id="filterMenu" class="dropdown-menu" aria-labelledby="dropdownMenuButton">
                <button id="filterAll" class="dropdown-item" v-on:click="getAlldrinks()">Alle</button>
                <button id="filterCocktails" class="dropdown-item" v-on:click="filterCocktails()">Cocktails</button>
                <button id="filterShots" class="dropdown-item" v-on:click="filterShots()">Shots</button>
                <button id="filterOrdinaryDrink" class="dropdown-item" v-on:click="filterOrdinaryDrink()">Ordinary Drink</button>
                <button id="filterPunch&party" class="dropdown-item" v-on:click="filterPunchParty()">Punch / Party drink</button>
                <button id="filterCoffeeTea" class="dropdown-item" v-on:click="filterCoffeeTea()">Coffee / Tea</button>
                <button id="filterBeer" class="dropdown-item" v-on:click="filterBeer()">Beer</button>
                <button id="filterAlcoholic" class="dropdown-item" v-on:click="filterAlcholic()">All Alcoholic Drinks</button>
                <button id="filterNonAlcoholic" class="dropdown-item" v-on:click="filterNonAlcholic()">All Non-Alcoholic Drinks</button>
            </div>
        </div>
    </div>

    <ul id="drinksListe" class="list-group" style="text-align: center;">
        <li class="list-Group-item mt-2" v-for="drink in drinks">
            <div class="row">
                <div class="col-3">
                </div>
                <div class="col-3 border-bottom" style="text-align: center;">
                    <div class="media">
                        <div class="media-left">
                            <div class="media-body">
                                <div class="drinkNavn">
                                    <h4 class="media-heading">{{drink.strDrink}}</h4>
                                </div>
                                Ingredienter: <span class="Indregient1">{{drink.strIngredient1}}</span> og <span
                                    class="Indregient2">{{drink.strIngredient2}}</span><br>
                                <span class="AlkoholJaNej"> <b>{{drink.strAlcoholic}}</b> </span>
                            </div>
                            <div class="mt-2">
                                <button type="button" class="btn border-black readButton">Læs mere</button>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-3 border-bottom">
                    <div class="media">
                        <div class="media-left">
                            <a href="#">
                                <img class="media-object mt-2 mb-2" id="listeBillede" :src="drink.strDrinkThumb"
                                    alt="..." style="height: 100px; width: 100px;">
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </li>
    </ul>  
</template>

<script>
import axios from 'axios';
const baseUrl = "https://www.thecocktaildb.com/api/json/v1/1/search.php?s="
export default{
    name: 'DrinkList',
    data() {
        return {
            alldrinks: [],
            drinks: [],
            drink: null,
            ingredients: [],
            ingredient: null,
        }
    },
    async created() {
        try {
            const response = await axios.get(baseUrl)
            this.drinks = await response.data.drinks
            this.sortByDrinksName(this.drinks)
            console.log(this.drinks)
            this.alldrinks = this.drinks

        } catch (ex) {
            alert(ex.message)
        }
    },
    methods: {
        getAlldrinks() {
            this.drinks = this.alldrinks
        },

        sortByDrinksName() {
            this.drinks.sort((drink1, drink2) =>
                drink1.strDrink.localeCompare(drink2.strDrink))
        },
        sortByDrinksNameR() {
            this.drinks.sort((drink1, drink2) =>
                drink2.strDrink.localeCompare(drink1.strDrink))
        },
        
        filterShots(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Shot"))
        },
        filterOrdinaryDrink (drinks) {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Ordinary Drink"))
        },
        filterCocktails(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Cocktail"))
        },
        filterBeer() {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Beer"))
        },
        filterCoffeeTea(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Coffee / Tea"))
        },
        filterPunchParty(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strCategory.includes("Punch / Party Drink"))
        },
        filterAlcholic(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strAlcoholic.includes("Alcoholic"))
        },
        filterNonAlcholic(drinks) {
            this.drinks = this.alldrinks.filter(b => b.strAlcoholic.includes("Non alcoholic"))
        },
    },
}
</script>