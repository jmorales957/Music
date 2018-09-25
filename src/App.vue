<template lang="pug">
#app
	h2 {{newTitle}}
	h1 Top en {{selectedCountry}}
	select(v-model="selectedCountry")
		option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
	spinner(v-show="loading")
	ul
		artists(v-for="artist in orderedArtist" :artist="artist" :key="artist.mbi")
</template>

<script>
import Spinner from './components/Spinner'
import Artists from './components/Artist'
import getArtists from './api'
export default {

  data () {
    return {
			title:'Musica',
      artists: [],
			countries:[
				{name:'Argentina', value:'argentina'},
				{name:'Colombia', value:'colombia'},
				{name:'España	' ,value:'spain'},
			],
			selectedCountry:'argentina',
			loading: true
    }
  },
	components:{
		Artists,
		Spinner
	},
	methods:{
		refreshArtist(){
			const self =this

			getArtists(this.selectedCountry)
			.then(function(artists){
				self.loading=false
				self.artists = artists
			})
			.catch(err => console.log(err))
		},
	},
	computed: {
		orderedArtist(){
			return this.artists.sort((a, b)=>((a.name > b.name) ? 1 : ((b.name > a.name) ? -1 : 0)))
		},
		newTitle(){
			return this.title.toUpperCase()
		}
	},
	mounted(){
		this.refreshArtist()
		console.log("asdasd")
	},
	watch:{
		selectedCountry(){
			this.refreshArtist()
		}
	}

}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color red !important
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
