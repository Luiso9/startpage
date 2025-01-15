<template>
	<div class="container">
		<div class="card_1">
			<div class="image">
				<p>{{ currentQuote }}</p>
				<div class="img"></div>
			</div>
		</div>
		<div class="card_2">
			<div id="txt">{{ hours }} : {{ minutes }}</div>
			<div class="secondsContainer">
				<div id="seconds">{{ seconds }}</div>
			</div>
		</div>
		<div class="card_3">
			<form action="https://www.google.com/search?q=a">
				<div class="search">
					<input
						type="search"
						name="q"
						aria-label="Search"
						placeholder="Search on Google"
					/>
					<i class="fa-solid fa-magnifying-glass iconSearch"></i>
				</div>
				<button type="submit" hidden>Search</button>
			</form>
		</div>
		<div class="card_4">
			<a class="link" href="https://www.youtube.com/">
				<p>Youtube</p>
				<i class="fa-brands fa-youtube icon"></i>
			</a>
			<a class="link" href="https://music.apple.com/">
				<p>Apple Music</p>
				<i class="fa-brands fa-apple icon"></i>
			</a>
			<a class="link" href="https://www.reddit.com/">
				<p>Reddit</p>
				<i class="fa-brands fa-reddit-alien icon"></i>
			</a>
			<a class="link" href="https://www.facebook.com/">
				<p>LinkedIn</p>
				<i class="fa-brands fa-facebook icon"></i>
			</a>
			<a class="link" href="https://github.com/">
				<p>Github</p>
				<i class="fa-brands fa-github-alt icon"></i>
			</a>
		</div>
    <LastFM />
	</div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from "vue";
import LastFM from "@/components/LastFM.vue";

export default {
	data() {
		return {
			hours: "00",
			minutes: "00",
			seconds: "00",
			timer: null,
		};
	},
	methods: {
		startTime() {
			const today = new Date();
			this.hours = today.getHours().toString().padStart(2, "0");
			this.minutes = today.getMinutes().toString().padStart(2, "0");
			this.seconds = today.getSeconds().toString().padStart(2, "0");

			this.timer = setTimeout(this.startTime, 500);
		},
	},
	mounted() {
		this.startTime();
	},
	beforeDestroy() {
		clearTimeout(this.timer);
	},
	components: {
		LastFM,
	},
	setup() {
		const quote = [
			"I hope it's come to no surprise.",
			"Spare my pride.",
			"In this gamble of time.",
			"Patience is a virtue",
			"I been on a dream.",
		];

		const currentQuote = ref(quote[0]);
		let index = 0;
		let timer;

		const changeQuote = () => {
			index = (index + 1) % quote.length;
			currentQuote.value = quote[index];
		};

		onMounted(() => {
			timer = setInterval(changeQuote, 240000);
		});

		onBeforeUnmount(() => {
			clearInterval(timer);
		});

		return { currentQuote };
	},
};
</script>
