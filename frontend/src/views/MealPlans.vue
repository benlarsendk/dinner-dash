<template>
    <div class="container">
        <div class="row">
            <div class="col">
                <h2>Meal plans</h2>
                <p>Here is a list of all your meal plans.</p>
                <p><span class="text-danger">NOTE:</span> This is a feature in the works, and might not work yet.</p>
            </div>
            <div class="col-auto">
                <router-link to="meal-plans/generate">
                    <button class="btn btn-success">Generate new meal plan</button>
                </router-link>
            </div>
        </div>
        <div class="row justify-content-center" v-if="recipes == null">
            <div class="spinner-border text-primary" role="status">
                <span class="sr-only">Loading...</span>
            </div>
        </div>
        <div class="row mt-4" v-if="!recipes || recipes.length === 0">
            <div class="col text-center">
                {{message}}
            </div>
        </div>
        <div class="row mb-4" v-for="i in rowCount" :key="i">
            <div class="col-12 mb-4 mb-sm-0 col-sm-3" v-for="recipe in itemsInRow(i)" :key="recipe.uid">
                <div class="card h-100">
                    <div class="card-body">
                        <h5 class="card-title">{{recipe.name}}</h5>
                        <p class="card-text">{{shortDesc(recipe.description)}}</p>
                    </div>
                    <div class="card-footer">
                        <div class="d-flex justify-content-between align-items-center">
                            <router-link :to="{name: 'show-recipe', params: { id: recipe.id }}">
                                <button class="btn btn-sm btn-success">More info</button>
                            </router-link>
                            <a class="badge badge-light remove-icon align-middle" @click="deleteRecipe(recipe.id, recipe.name)">
                                <i class="fas fa-times"></i>
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "MealPlans"
    }
</script>

<style scoped>

</style>