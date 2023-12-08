<template>
    <nav v-if="currentPageCustom == 'CustomDrinksList'" class="navbar navbar-expand-sm navbar-dark bg-dark" aria-label="Third navbar example">
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
                        <button id="filterAll" class="dropdown-item" v-on:click="getAlldrinksCustom()">Alle</button>
                        <button id="filterCocktails" class="dropdown-item" v-on:click="filterCocktails()">Cocktails</button>
                        <button id="filterShots" class="dropdown-item" v-on:click="filterShots()">Shots</button>
                        <button id="filterOrdinaryDrink" class="dropdown-item" v-on:click="filterOrdinaryDrink()">Ordinary
                            Drink</button>
                        <button id="filterPunch&party" class="dropdown-item" v-on:click="filterPunchParty()">Punch &
                            Party drinkCustom</button>
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
            <input id="searchBarNavn" v-model="searchToGetByCustom" placeholder="Drink Navn" type="text"
                v-on:keyup.enter="getByNameCustom(searchToGetByCustom)" />
            <input id="searchBarIngredient" v-model="searchToGetByForIngredientCustom" placeholder="Ingredient" type="text"
                v-on:keyup.enter="getByIngredient(searchToGetByForIngredientCustom)" />
        </div>
    </nav>
    <div v-if="currentPageCustom == 'CustomDrinksList'" class="mt-1 mt-5" style="text-align: center;">
        <button type="button" class="btn border-black" id="sortByAsc" v-on:click="sortByDrinksName()">↑</button>
        <button type="button" class="btn border-black" id="sortByDsc" v-on:click="sortByDrinksNameR()">↓</button>
    </div>


    <ul id="drinksListe" v-if="currentPageCustom == 'CustomDrinksList'" class="list-group" style="text-align: center;">
        <li class="list-Group-item mt-2" v-for="drinkCustom in drinksCustom">
            <div class="row">
                <div class="col-3">
                </div>
                <div class="col-3 border-bottom" style="text-align: center;">
                    <div class="media">
                        <div class="media-left">
                            <div class="media-body">
                                <div class="drinkNavn">
                                    <h4 class="media-heading">{{ drinkCustom.strDrink }}</h4>
                                </div>
                                Ingredienter: <span class="Indregient1">{{ drinkCustom.strIngredient1 }}</span> og <span
                                    class="Indregient2">{{ drinkCustom.strIngredient2 }}</span><br>
                                <span class="AlkoholJaNej"> <b>{{ drinkCustom.strAlcoholic }}</b> </span>
                            </div>
                            <div class="mt-2">
                                <button type="button" class="btn border-black readButton" @click="setDrink(drinkCustom) + setCurrentPage('DrinkItem');" >Læs
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
                                <img class="media-object mt-2 mb-2" id="listeBillede" :src="drinkCustom.strDrinkThumb" alt="..."
                                    style="height: 100px; width: 100px;">
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </li>
    </ul>
    <button v-if="currentPageCustom == 'DrinkItem'" type="button" class="btn border-black readButton" @click="setCurrentPage('CustomDrinksList') + setDrinkNull()">tilbage</button>
    <DrinkItem v-if="currentPageCustom == 'DrinkItem'" :drinkCustom="currentDrinkCustom" />
</template>

<script>
import axios from 'axios';
import DrinkItem from './DrinkItem.vue';
import CreateDrink from './CreateDrink.vue'
const baseUrlCustom = "http://localhost:5002/api/DrinkModel"
export default {
    name: 'CustomDrinksList',
    data() {
        return {
            alldrinksCustom: [],
            drinksCustom: [],
            drinkCustom: null,
            ingredientsCustom: [],
            ingredientCustom: null,
            currentDrinkCustom: null,
            searchToGetByCustom: "",
            searchToGetByForIngredientCustom: "",
            currentPageCustom: 'CustomDrinksList',
        };
    },
    async created() {
        try {
            const responseCustom = await axios.get(baseUrlCustom);
            console.log(baseUrlCustom)
            this.drinksCustom = await responseCustom.data;
            this.sortByDrinksName(this.drinksCustom);
            console.log(this.drinksCustom);
            this.alldrinksCustom = this.drinksCustom;
        }
        catch (ex) {
            alert(ex.message);
        }
    },
    methods: {
        setDrink(drinkCustom) {
            this.currentDrinkCustom = drinkCustom;
        },
        setCurrentPage(page){
            this.currentPageCustom = page
        },
        setDrinkNull() {
            this.currentDrinkCustom = null;
        },
        getAlldrinksCustom() {
            this.drinksCustom = this.alldrinksCustom;
        },
        sortByDrinksName() {
            this.drinksCustom.sort((drink1, drink2) => drink1.strDrink.localeCompare(drink2.strDrink));
        },
        sortByDrinksNameR() {
            this.drinksCustom.sort((drink1, drink2) => drink2.strDrink.localeCompare(drink1.strDrink));
        },
        filterShots(drinksCustom) {
            this.drinksCustom = this.alldrinksCustom.filter(b => b.strCategory.includes("Shot"));
        },
        filterOrdinaryDrink(drinksCustom) {
            this.drinksCustom = this.alldrinksCustom.filter(b => b.strCategory.includes("Ordinary Drink"));
        },
        filterCocktails(drinksCustom) {
            this.drinksCustom = this.alldrinksCustom.filter(b => b.strCategory.includes("Cocktail"));
        },
        filterBeer() {
            this.drinksCustom = this.alldrinksCustom.filter(b => b.strCategory.includes("Beer"));
        },
        filterCoffeeTea(drinksCustom) {
            this.drinksCustom = this.alldrinksCustom.filter(b => b.strCategory.includes("Coffee / Tea"));
        },
        filterPunchParty(drinksCustom) {
            this.drinksCustom = this.alldrinksCustom.filter(b => b.strCategory.includes("Punch / Party Drink"));
        },
        filterAlcholic(drinksCustom) {
            this.drinksCustom = this.alldrinksCustom.filter(b => b.strAlcoholic.includes("Alcoholic"));
        },
        filterNonAlcholic(drinksCustom) {
            this.drinksCustom = this.alldrinksCustom.filter(b => b.strAlcoholic.includes("Non alcoholic"));
        },
        getByNameCustom(search) {
            const urlCustom = baseUrlCustom + "?name=" + search

            console.log(urlCustom)
            this.helperGetAndShow(urlCustom)
        },
        getByIngredient(search) {
            if (search != null) {
                this.drinksCustom = this.alldrinksCustom.filter(b => b.strIngredient1.includes(search) || b.strIngredient2.includes(search))
                return this.drinksCustom
            }
            else {
                return "No drinksCustom found"
            }
        },
        async helperGetAndShow(urlCustom) {
            try {
                const responseCustom = await axios.get(urlCustom)
                console.log(urlCustom)
                this.drinksCustom = await responseCustom.data.drinksCustom
            } catch (ex) {
                alert(ex.message)
            }
        },
    },
    components: { DrinkItem, CreateDrink, }
    

}
</script>