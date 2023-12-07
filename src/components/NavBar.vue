<template>
    <nav v-if="currentPage=='liste'" class="navbar navbar-expand-sm navbar-dark bg-dark" aria-label="Third navbar example">
        <div class="container-fluid">
            <a class="navbar-brand" href="">Drinks</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarsExample03"
                aria-controls="navbarsExample03" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="container mt-1">
                <div class="dropdown" style="text-align: left;">
                    <button class="btn btn-dark dropdown-toggle" type="button" id="dropdownMenuButton"
                        data-bs-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                        Vælg et filter
                    </button>
                    <div id="filterMenu" class="dropdown-menu" aria-labelledby="dropdownMenuButton">
                        <button id="filterAll" class="dropdown-item" v-on:click="getAlldrinks()">Alle</button>
                        <button id="filterCocktails" class="dropdown-item" v-on:click="filterCocktails()">Cocktails</button>
                        <button id="filterShots" class="dropdown-item" v-on:click="filterShots()">Shots</button>
                        <button id="filterOrdinaryDrink" class="dropdown-item" v-on:click="filterOrdinaryDrink()">Ordinary
                            Drink</button>
                        <button id="filterPunch&party" class="dropdown-item" v-on:click="filterPunchParty()">Punch &
                            Party drink</button>
                        <button id="filterCoffeeTea" class="dropdown-item" v-on:click="filterCoffeeTea()">Coffee &
                            Tea</button>
                        <button id="filterBeer" class="dropdown-item" v-on:click="filterBeer()">Beer</button>
                        <button id="filterAlcoholic" class="dropdown-item" v-on:click="filterAlcholic()">All
                            Alcoholic Drinks</button>
                        <button id="filterNonAlcoholic" class="dropdown-item" v-on:click="filterNonAlcholic()">All
                            Non-Alcoholic Drinks</button>
                    </div>
                </div>
            </div>
            <input id="searchBarNavn" v-model="searchToGetBy" placeholder="Drink Navn" type="text"
                v-on:keyup.enter="getByName(searchToGetBy)" />
            <input id="searchBarIngredient" v-model="searchToGetByForIngredient" placeholder="Ingredient" type="text"
                v-on:keyup.enter="getByIngredient(searchToGetByForIngredient)" />
        </div>
    </nav>
</template>

<script>
import axios from 'axios';
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
    
    

}
</script>