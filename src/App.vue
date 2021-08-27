<template>
	<div class="content">
		<div class="header">
			<div class="avatar-wrapper">
				<img srcset="./assets/headshot180.jpeg 180w, ./assets/headshot90.jpeg 90w" sizes="90px" class="avatar">
			</div>
			<div class="name">
				<h1 class="heading">
					Daniel Varnau
				</h1>
				<p class="title">
					web developer
				</p>
			</div>
			<div
				ref="nav"
				class="nav-wrapper"
			>
				<nav>
					<ul role="tablist" class="nav">
						<li class="nav-item">
							<button
								class="nav-button"
								ref="about"
								role="tab"
								aria-controls="about"
								:aria-selected="tab === 'about'"
								@click="changeTab('about')"
							>
								About
							</button>
						</li>
						<li class="nav-item">
							<button
								class="nav-button"
								ref="work"
								role="tab"
								aria-controls="work"
								:aria-selected="tab === 'work'"
								@click="changeTab('work')"
							>
								Work
							</button>
						</li>
						<li class="nav-item">
							<button
								class="nav-button"
								ref="contact"
								role="tab"
								aria-controls="contact"
								:aria-selected="tab === 'contact'"
								@click="changeTab('contact')"
							>
								Contact
							</button>
						</li>
					</ul>
				</nav>
				<div
					class="nav-selection"
					:style="selection"
				/>
			</div>
		</div>
		<div
			v-if="tab === 'about'"
			id="about"
			role="tabpanel"
			:aria-hidden="tab !== 'about'"
			:aria-expanded="tab === 'about'"
			class="body"
		>
			<p>
				I'm a front-end-focused web developer from Nashville, TN. I graduated from Purdue University in 2012
				with a degree in acoustical engineering. After working in the field for about a year,
				I realized it wasn't really what I wanted to be doing, so I started learning web development,
				which I loved pretty immediately. To me, web development is the perfect mix of left and right brain.
				It's both visual and logical. While I didn't love the work, I feel that engineering school did give me a great mind for problem
				solving that has translated very well to development.
			</p>
			<p>
				I like to stay busy in my free time. When I'm not working, you can find me obsessing over music,
				disc golfing, exploring dungeons, and slaying dragons. A lot of the time, any of those activities
				will also be accompanied by a good craft beer.
			</p>
			<p></p>
		</div>
		<div
			v-if="tab === 'work'"
			id="work"
			role="tabpanel"
			:aria-hidden="tab !== 'work'"
			:aria-expanded="tab === 'work'"
			class="body"
		>
			<p>
				I'm currently a senior developer at <a href="https://lewiscommunications.com">Lewis Communications</a>.
			</p>
			<p>
				I've worked on projects of all sizes including <a href="https://www.usahealthsystem.com">USA Health</a>,
				<a href="https://www.goodpeoplebrewing.com">Good People Brewing</a>, and the not-quite-yet-launched
				<a href="https://www.whitetailproperties.com">Whitetail Properties</a> rebuild.
				I've won several regional ADDY awards over the years including Best of Interactive for
				<a href="https://promotions.abeka.com/christmas-card">Abeka's Christmas card builder</a>.
			</p>
			<p>
				I'm especially focused on responsive layouts, user interfaces, and accessibility.
			</p>
		</div>
		<div
			v-if="tab === 'contact'"
			id="contact"
			role="tabpanel"
			:aria-hidden="tab !== 'contact'"
			:aria-expanded="tab === 'contact'"
			class="body"
		>
			<p>
				<a href="mailto:djvarnau@gmail.com">Email Me</a>
			</p>
			<p>
				<a href="https://www.linkedin.com/in/djvarnau/">LinkedIn</a>
			</p>
			<p>
				<a href="https://drive.google.com/file/d/1eamwbDZ5N9Dk_0PR0dnxJo8x3y0CO_kS/view?usp=sharing">Resume</a>
			</p>
		</div>
	</div>
</template>

<script>
import debounce from 'lodash/debounce';

export default {
	name: 'App',
	data: () => ({
		tab: 'about',
		selection: {
			display: 'none',
			left: 0,
			width: '5.1rem',
		},
	}),
	mounted() {
		const segs = window.location.pathname.split('/')
		const tab = segs[1];
		const allowed = ['contact', 'work'];

		if (tab.length && allowed.includes(tab)) {
			this.tab = tab;
		}

		window.onload = () => {
			this.changeTab(this.tab, segs.length > 2 || !allowed.includes(tab));
		}

		window.addEventListener('resize', debounce(() => { this.changeTab() }, 50));
	},
	methods: {
		changeTab(tab = this.tab, updateUrl = true) {
			const button = this.$refs[tab];
			this.tab = tab;

			this.selection.display = 'block';
			this.selection.width = `${button.offsetWidth}px`;
			this.selection.left = `${button.getBoundingClientRect().left - this.$refs.nav.getBoundingClientRect().left}px`;

			if (updateUrl) {
				window.history.pushState({}, '', `/${this.tab !== 'about' ? this.tab : '' }`);
			}
		},
	},
}
</script>

<style>
@font-face {
	font-family: 'ClearfaceBold';
	src:  url('./fonts/clearface-bold.woff2') format('woff2'),
	url('./fonts/clearface-bold.woff') format('woff');
}

* {
	box-sizing: border-box;
}

*::selection {
	background: var(--green); /* WebKit/Blink Browsers */
	color: var(--cream);
}

*::-moz-selection {
	background: var(--green); /* WebKit/Blink Browsers */
	color: var(--cream);
}

:root {
	--green: #365747;
	--cream: #fff5eb;
	--black: #464646;
}

html {
	background-color: var(--green);
	font-size: 62.5%;
}

body {
	background-color: var(--cream);
	min-height: 100vh;
	margin: 0;
	width: 100vw;
}

.content {
	margin: 0 auto;
	max-width: 160rem;
	min-height: 100vh;
	padding: 6rem 6%;
}

.avatar-wrapper {
	border: 4px solid var(--green);
	height: 10rem;
	margin: 0 auto 1rem;
	overflow: hidden;
	width: 10rem;
}

.avatar {
	height: 100%;
	max-width: none;
}

.name {
	text-align: center;
}

.heading {
	color: var(--green);
	font-family: 'ClearfaceBold', serif;
	font-size: 4rem;
	line-height: 1em;
	margin: 0 0 .4rem;
}

.title {
	color: var(--black);
	font-family: 'Rubik', sans-serif;
	font-size: 1.6rem;
	margin: 0 0 2rem;
}

.nav-wrapper {
	position: relative;
	margin: 0 auto;
}

.nav-selection {
	background-color: var(--green);
	bottom: -1rem;
	content: '';
	display: none;
	height: .5rem;
	position: absolute;
	left: 0;
	transition: width .1s linear, left .1s ease-in-out;
	width: 4.7rem;
}

.nav {
	list-style-type: none;
	display: flex;
	justify-content: center;
	margin: 0 auto;
	padding: 0;
}

.nav-item {
	display: inline-block;
	padding: 0 1rem;
}

.nav-button {
	appearance: none;
	background: transparent;
	border: none;
	color: var(--black);
	cursor: pointer;
	font-size: 1.8rem;
	font-family: 'Rubik', sans-serif;
	padding: 0;
	transition: color .25s ease-in-out;
}

.nav-button:focus,
a:focus {
	outline-color: var(--green);
}

.nav-button:hover {
	color: var(--green);
}

.body {
	margin: 3rem auto 0;
	max-width: 69rem;
}

.body > p {
	color: var(--black);
	font-size: 1.8rem;
	font-family: 'Rubik', sans-serif;
	line-height: 1.4em;
}

.body a {
	color: var(--black);
	transition: color .25s;
}

.body a:hover {
	color: #045c15;
	text-decoration: none;
}

@media (min-width: 999px) {
	.header {
		align-items: flex-end;
		display: flex;
		justify-content: flex-start;
	}

	.content {
		padding: 20vh 10%;
	}

	.name {
		text-align: left;
	}

	.avatar-wrapper {
		border-width: 5px;
		margin: 0 2rem 0 0;
	}

	.heading {
		font-size: 6rem;
		line-height: .7em;
	}

	.title {
		margin-bottom: 0;
		font-size: 2.6rem;
	}

	.nav-wrapper {
		margin: 0 0 0 auto;
	}

	.body {
		margin: 3rem 0 0;
	}

	.body > p {
		font-size: 2rem;
	}

	.nav-item {
		margin-right: 1.4rem;
	}
}
</style>
