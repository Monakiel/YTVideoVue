<template>
	<div class="container-fluid">
		<Header />
		<SearchBar @termChange="onTermChange"></SearchBar>
		<div class="row mx-3">
			<VideoDetail class="pr-1" :video="selectedVideo" />
			<VideoList class="pl-1" @videoSelect="onVideoSelect" :videos="videos"></VideoList>
		</div>
	</div>
</template>

<script>
	import axios from 'axios'
	import "core-js/stable"
	import "regenerator-runtime/runtime"
	import Header from './components/Header'
	import SearchBar from './components/SearchBar'
	import VideoList from './components/VideoList'
	import VideoDetail from './components/VideoDetail'

	const API_KEY = 'YOUR_YT_API_KEY'

	export default {
		name: 'App',
		components: {
			Header,
			SearchBar,
			VideoList,
			VideoDetail
		},
		data() {
			return {
				videos: [],
				selectedVideo: null
			}
		},
		methods: {
			async onTermChange(searchTerm) {

				const response = await axios.get('https://www.googleapis.com/youtube/v3/search', {
					params: {
						key: API_KEY,
						type: 'video',
						part: 'snippet',
						q: searchTerm
					}
				})
				this.videos = response.data.items
			},

			onVideoSelect(video) {
				this.selectedVideo = video
			}
		}
	}
</script>

<style scoped>
	* {
		box-sizing: border-box;
		margin: 0;
		padding: 0;
	}

	body {
		font-family: Arial, Helvetica, sans-serif;
		line-height: 1.4;
	}
</style>