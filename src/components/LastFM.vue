<template>
	<div class="card_5">
		<div
			class="card_content"
			v-for="(track, index) in recentTracks"
			:key="track.url || index"
		>
			<img
				class="card_avatar_img"
				:src="getImage(track.image, 'large')"
				alt="Avatar"
				loading="lazy"
			/>
			<div class="song_detail">
				<h1 class="song_name">{{ track.name }}</h1>
				<span class="artist">{{ track.artist["#text"] }}</span>
			</div>
		</div>
	</div>
</template>

<script>
import axios from "axios";

export default {
	data() {
		return {
			recentTracks: [],
		};
	},
	methods: {
		async fetchRecentTracks() {
			try {
				const apiKey = import.meta.env.VITE_API_TOKEN;
				const response = await axios.get(
					`https://ws.audioscrobbler.com/2.0/?method=user.getrecenttracks&user=rj&api_key=${apiKey}&format=json&user=snoozief9ur&limit=4`
				);

				this.recentTracks = Array.isArray(response.data.recenttracks.track)
					? response.data.recenttracks.track
					: [response.data.recenttracks.track];
			} catch (error) {
				console.error("Error fetching recent tracks:", error);
			}
		},
		getImage(images, size) {
			const imageObj = images.find((img) => img.size === size);
			return imageObj ? imageObj["#text"] : "";
		},
	},
	mounted() {
		this.fetchRecentTracks();
	},
};
</script>
