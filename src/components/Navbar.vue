<template>
    <div class="navbar">
        <nav class="nav">
            <div class="container">
                <router-link :to="{name: 'Home'}" class="brand-logo left"><i><img src="../assets/logo_scaled.svg" width="60" height="60"></i> Lebillion</router-link>

                <ul class="right">
                    <li><router-link :to="{name: 'About'}">About</router-link></li>
                    <li v-if="!user"><router-link :to="{name: 'Signup'}">Signup</router-link></li>
                    <li v-if="!user"><router-link :to="{name : 'Login'}">Login</router-link></li>
                    <li v-if="user"><router-link :to="{name: 'ViewProfile', params: {slug: currentUserSlug}}">{{ user.displayName }}</router-link></li>
                    <li v-if="user"><a @click.prevent="logout()">Logout</a></li>
                </ul>
                
            </div>
        </nav>
    </div>
</template>

<script>
import firebase from 'firebase'
import slugify from 'slugify'
export default {
    name: 'Navbar',
    data() {
        return {
            user: null,
        }
    },
    computed:{
        currentUserSlug(){
            return this.computeSlug()
        }
    },
    methods: {
        computeSlug(){
            if(this.user.displayName){
                return slugify(this.user.displayName, {
                        replacement: '-',
                        remove: /[$*_+~.()'"!\-:@]/g,
                        lower: true,
                    }) 
            }else {
                return 'null'
            }
        },
        logout(){
            firebase.auth().signOut()
            .then(() => this.$router.push({name: 'Login'}))
            .catch(err => console.log(err))
        }
    },
    created(){
        firebase.auth().onAuthStateChanged(user => {
            if(user){
                this.user = user
            } else {
                this.user = null
            }
        })
        
    }
}
</script>

<style>
    .navbar .nav{
        background-color: #CAEBF2;
    }
    .navbar ul a{
        color: #111;
    }
    .navbar .brand-logo{
        color: #111
    }
</style>
