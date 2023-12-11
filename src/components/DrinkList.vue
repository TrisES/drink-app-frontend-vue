<template>
    <nav v-if="currentPage == 'liste'" class="navbar navbar-expand-sm navbar-dark bg-dark" aria-label="Third navbar example">
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
                        Pick a Filter
                    </button>
                    <div id="filterMenu" class="dropdown-menu" aria-labelledby="dropdownMenuButton">
                        <button id="filterAll" class="dropdown-item" v-on:click="getAlldrinks()">All</button>
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
            <input id="searchBarNavn" v-model="searchToGetBy" placeholder="Drink Name" type="text"
                v-on:keyup.enter="getByName(searchToGetBy)" />
            <input id="searchBarIngredient" v-model="searchToGetByForIngredient" placeholder="Ingredient" type="text"
                v-on:keyup.enter="getByIngredient(searchToGetByForIngredient)" />
        </div>
    </nav>
    <div class="text-center">
        <button v-if="currentPage == 'liste'" type="button" class="btn btn-success w-50" @click="setCurrentPage('CreateDrink')">Create Drink</button>
    </div>
    <button v-if="currentPage == 'CreateDrink'" type="button" class="btn border-black readButton" @click="setCurrentPage('liste')">tilbage</button>
    <CreateDrink v-if="currentPage == 'CreateDrink'"></CreateDrink>

    <CustomDrinksList v-if="currentPage == 'CustomDrinkList'" :drinks="drinks" />
    <div class="text-center">
        <button v-if="currentPage == 'liste'" type="button" class="btn btn-success w-50" @click="setCurrentPage('CustomDrinkList')">custom drink list</button>
    </div>
    

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
                                <!-- Ingredienter: <span class="Indregient1">{{ drink.strIngredient1 }}</span> og <span
                                    class="Indregient2">{{ drink.strIngredient2 }}</span><br> -->
                                <!-- Tags: <span class="Tags">{{ drink.strTags }}</span><br> -->
                                Category: <span class="Kategori">{{ drink.strCategory }}</span><br>
                                <span class="AlkoholJaNej"> <b>{{ drink.strAlcoholic }}</b> </span>
                            </div>
                            <div class="mt-2">
                                <button type="button" class="btn btn-primary" @click="setDrink(drink) + setCurrentPage('DrinkItem')">Details</button>
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
    <button v-if="currentPage == 'DrinkItem'" type="button" class="btn btn-danger" @click="setCurrentPage('liste') + setDrinkNull()">Tilbage</button>
    <DrinkItem v-if="currentPage == 'DrinkItem'" :drink="currentDrink" />
</template>

<script>
import axios from 'axios';
import DrinkItem from './DrinkItem.vue';
import CreateDrink from './CreateDrink.vue'
import CustomDrinksList from './CustomDrinksList.vue'
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
    components: { DrinkItem, CreateDrink, CustomDrinksList }
    

}
</script>