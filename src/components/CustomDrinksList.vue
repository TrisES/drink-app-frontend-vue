<template>
    <nav v-if="currentPage == 'CustomDrinksList'" class="navbar navbar-expand-sm navbar-dark bg-dark"
        aria-label="Third navbar example">
        <div class="container-fluid">
            <a class="navbar-brand" href="">Drinks</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarsExample03"
                aria-controls="navbarsExample03" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <!--Alle filtre på drinks, så man kan filtrere efter hvad man leder efter. DVS shots, cocktails osv. -->
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
            <!--Søge funktionen som gør at du kan søge på forskellige drinks, baseret på deres navn-->
            <input id="searchBarIngredient" v-model="searchToGetByForIngredientCustom" placeholder="Search by Ingredient"
                type="text" v-on:keyup.enter="getByIngredient(searchToGetByForIngredientCustom)" />
        </div>
    </nav>
    <!--Sorterings knapper og Create knap, så man kan oprette nye drinks.-->
    <div class="text-center">
        <button v-if="currentPage == 'CustomDrinksList'" type="button" class="btn btn-success w-50"
            @click="setCurrentPage('CreateDrink')">Create Drink</button>
    </div>

    <button v-if="currentPage == 'CreateDrink'" type="button" class="btn btn-danger"
        @click="setCurrentPage('CustomDrinksList')">Tilbage</button>
    <CreateDrink v-if="currentPage == 'CreateDrink'"></CreateDrink>

    <div v-if="currentPage == 'CustomDrinksList'" class="mt-1 mt-5" style="text-align: center;">
        <button type="button" class="btn border-black" id="sortByAsc" v-on:click="sortByDrinksName()">↑</button>
        <button type="button" class="btn border-black" id="sortByDsc" v-on:click="sortByDrinksNameR()">↓</button>
    </div>

    <!--Displayer alle drinks-->
    <ul id="drinksListe" v-if="currentPage == 'CustomDrinksList'" class="list-group" style="text-align: center;">
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
                                Ingredienser: <span class="Indregient1">{{ drink.strIngredient1 }}</span> og <span
                                    class="Indregient2">{{ drink.strIngredient2 }}</span><br>
                                <span class="AlkoholJaNej"> <b>{{ drink.strAlcoholic }}</b> </span>
                            </div>
                            <div class="mt-2">
                                <button type="button" class="btn btn-primary"
                                    @click="setDrink(drink) + setCurrentPage('DrinkItem')">Details</button>


                                <button type="button" class="btn btn-warning"
                                    @click="setDrink(drink) + setCurrentPage('UpdateDrink');">Update</button>


                                <button type="button" class="btn btn-danger" @click="deleteDrink(drink);">Delete</button>
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
    <!--Tilbage knapper fra den enkelte side man er i-->
    <button v-if="currentPage == 'DrinkItem'" type="button" class="btn btn-danger"
        @click="setCurrentPage('CustomDrinksList') + setDrinkNull()">Tilbage</button>
    <DrinkItem v-if="currentPage == 'DrinkItem'" :drink="currentDrink" />
    <button v-if="currentPage == 'UpdateDrink'" type="button" class="btn btn-danger"
        @click="setCurrentPage('CustomDrinksList')">Tilbage</button>
    <UpdateDrink v-if="currentPage == 'UpdateDrink'" :idToUpdate="currentDrink.id" :drinkToUpdate="currentDrink">
    </UpdateDrink>
</template>

<script>
import axios from 'axios';
import DrinkItem from './DrinkItem.vue';
import CreateDrink from './CreateDrink.vue'
import UpdateDrink from './UpdateDrink.vue'
const baseUrl = "https://drinksmaskinerest.azurewebsites.net/api/DrinkModel"
export default {
    name: 'CustomDrinksList',
    data() {
        return {
            alldrinks: [],
            drinks: [],
            drink: null,
            ingredientsCustom: [],
            ingredientCustom: null,
            currentDrink: null,
            searchToGetByCustom: "",
            searchToGetByForIngredientCustom: "",
            currentPage: 'CustomDrinksList',
        };
    },
    async created() {
        try {
            const responseCustom = await axios.get(baseUrl);
            console.log(baseUrl)
            this.drinks = await responseCustom.data;
            this.sortByDrinksName(this.drinks);
            console.log(this.drinks);
            this.alldrinks = this.drinks;
        }
        catch (ex) {
            alert(ex.message);
        }
    },
    methods: {
        //Metoder til at sortere drinks, filtrere drinks, slette drinks samt at se alle drinks 
        setDrink(drink) {
            this.currentDrink = drink;
        },
        setCurrentPage(page) {
            this.currentPage = page
        },
        setDrinkNull() {
            this.currentDrink = null;
        },
        getAlldrinksCustom() {
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
        // getByNameCustom(search) {
        //     const urlCustom = baseUrl + "?name=" + search

        //     console.log(urlCustom)
        //     this.helperGetAndShow(urlCustom)
        // },
        getByIngredient(search) {
            if (search != null) {
                this.drinks = this.alldrinks.filter(b => b.strIngredient1.includes(search) || b.strIngredient2.includes(search))
                return this.drinks
            }
            else {
                return "No drinks found"
            }
        },
        async deleteDrink(drink) {
            this.currentDrink = drink
            if (drink && drink.id) {
                try {
                    const urlCustom = baseUrl + "/" + this.currentDrink.id
                    console.log(this.currentDrink.id)
                    console.log(urlCustom)
                    let responseDel = await axios.delete(urlCustom)
                    this.drinks = this.drinks.filter(b => b.id != this.currentDrink.idD)
                    if (responseDel.status == 200) {
                        alert("Drink Deleted")
                    }
                    if (responseDel.status == 400) {
                        alert("Drink not Deleted")
                    }
                } catch (exPut) {
                    console.log(exPut.message)
                    alert("Drink not deleted. \nException: " + exPut.message)
                }

            }


        },
        // async helperGetAndShow(urlCustom) {
        //     try {
        //         const responseCustom = await axios.get(urlCustom)
        //         console.log(urlCustom)
        //         this.drinks = await responseCustom.data.drinks
        //     } catch (ex) {
        //         alert(ex.message)
        //     }
        // },
    },
    components: { DrinkItem, CreateDrink, UpdateDrink }


}
</script>