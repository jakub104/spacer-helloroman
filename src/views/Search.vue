<template>
	<div class="wrapper">
		<div class="search">
			<label for="search">Search</label>
			<input
				id="search"
				name="search"
				v-model="searchValue"
				@input="handleInput"
			/>
			<ul>
				<li v-for="result in results" :key="result.data[0].nasa_id">
					<h3>{{result.data[0].title}}</h3>
					<img
						class="image"
						:src="result.links[0].href"
						:alt="result.data[0].title"
						:title="result.data[0].title"
					/>
					<p>{{result.data[0].title}}</p>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
import axios from 'axios'
import debounce from 'lodash.debounce'

const API = 'https://images-api.nasa.gov/search';
export default {
	name: 'Search',
	data() {
		return {
			searchValue: '',
			results: []
		}
	},
	methods: {
		handleInput: debounce(function() {
			axios.get(`${API}?q=${this.searchValue}&media_type=image`).then(res => {
				this.results = res.data.collection.items
			}).catch(err => {
				console.log(err)
			})
		}, 500)
	}
}
</script>

<style lang="scss" scoped>
	.wrapper {
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 0;
		padding: 30px;
	}
	.search {
		width: 250px;
		display: flex;
		flex-direction: column;
		label {
			font-family: 'Ubuntu', sans-serif;
		}
		input {
			height: 30px;
			border: none;
			border-bottom: 1px solid black;
		}
	}
	.image {
		width: 500px;
	}
</style>