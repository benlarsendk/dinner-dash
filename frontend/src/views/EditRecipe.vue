<template>
    <div class="container">
        <div class="row justify-content-sm-center mb-4">
            <div class="col col-sm-8 mb-2" v-if="loaded">
                <h2>Edit recipe</h2>
                <div class="alert alert-danger" v-if="error">{{error}}</div>
                <form @submit.prevent="editRecipe">
                    <div class="form-group">
                        <label for="name">Recipe name</label>
                        <input type="text" v-model="name" class="form-control" id="name"
                               placeholder="Enter a name for your recipe" autocomplete="off">
                    </div>
                    <div class="form-group">
                        <label for="url">Recipe link</label>
                        <input type="url" v-model="url" class="form-control" id="url"
                               placeholder="http://recipeplace.com/somerecipe">
                    </div>
                    <div class="form-group">
                        <label for="description">Description</label>
                        <textarea id="description" rows="10" class="form-control" v-model="description"></textarea>
                    </div>
                    <div class="form-group">
                        <label for="directions">Directions</label>
                        <textarea id="directions" rows="10" class="form-control" v-model="directions"></textarea>
                    </div>
                    <div class="float-right">
                        <button type="button" class="btn btn-secondary" @click="cancel">Cancel</button>&nbsp;
                        <button type="submit" class="btn btn-success">Submit</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    import {backendService} from "../services/BackendService"

    export default {
        name: "edit-recipe",
        data: function () {
            return {
                id: this.$route.params.id,
                name: null,
                url: null,
                description: null,
                loaded: false,
                error: null,
                directions: null
            }
        },
        mounted() {
            backendService.getRecipe(this.id).then(recipe => {
                this.error = null;
                this.name = recipe.name;
                this.url = recipe.url;
                this.description = recipe.description;
                this.directions = recipe.directions;
                this.loaded = true
            }).catch(err => {
                this.error = err
            })
        },
        methods: {
            editRecipe() {
                let recipe = {
                    name: this.name,
                    url: this.url,
                    description: this.description,
                    directions: this.directions
                };
                backendService.updateRecipe(this.id, recipe)
                    .then(() => this.$router.push("/recipes/" + this.id))
                    .catch(err => {
                        this.error = err
                    });
            },
            updateDescription(data) {
                this.description = data
            },
            cancel() {
                this.$router.go(-1)
            }
        }
    }
</script>
