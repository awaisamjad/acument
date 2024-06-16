<script>
	import { onMount } from 'svelte';
	// import { marked } from 'marked';

	let title = '';
	let category = '';
	let date = '';
	let location = '';
	let rolesAndResponsibilities = '';

	try {
		onMount(async () => {
			await fetch('http://localhost:1337/api/jobs', {
				method: 'GET',
				headers: {
					'Content-Type': 'application/json'
				}
			})
				.then((response) => response.json())
				.then((json_response) => {
					console.log('Response JSON', json_response);
					console.log('Response JSON Attributes', json_response.data[0].attributes.title);

					const job = json_response.data[0];
					const attributes = job.attributes;

					title = attributes.title;
					category = attributes.category;
					date = attributes.date;
					location = attributes.location;
					rolesAndResponsibilities = attributes.roles_and_responsibilities;

					console.log('title: ', title);
				});
		});
	} catch (error) {
		console.log('Job API Error');
		console.error(error);
	}

	function formatDate(dateString) {
		// Parse the date string into a Date object
		const date = new Date(dateString);

		// Define options for toLocaleDateString
		const options = { day: 'numeric', month: 'short', year: 'numeric' };

		// Format the date using toLocaleDateString
		return date.toLocaleDateString('en-GB', options);
	}
</script>

<div class="container">
	<!-- Title -->
	<div class="title">
		<h1>{title}</h1>
	</div>

	<!-- Apply Button -->
	<div class="apply-button">
		<a href="/apply">Apply</a>
	</div>

	<!-- Category -->
	<div class="category">
		<p>Category: {category}</p>
	</div>

	<!-- Roles and Responsibilities -->
	<div class="roles-and-responsibilities">
		<!-- {@html marked(rolesAndResponsibilities)} -->
		{rolesAndResponsibilities}
	</div>

	<!-- Location -->
	<div class="location">
		<p>Location: {location}</p>
	</div>

	<!-- Date -->
	<div class="date">
		<p>Date: {formatDate(date)}</p>
	</div>
</div>

<style lang="scss">
	body {
		font-family: Arial, sans-serif;
		color: #333;
		background-color: #f4f4f4;
		margin: 0;
		padding: 0;
	}

	div {
		&.date {
			p {
				color: #ffbbbb;
				font-size: 1.2em;
				font-weight: bold;
			}
		}
	}

	h1,
	h2,
	h3,
	h4,
	h5,
	h6 {
		color: #444;
	}

	p {
		line-height: 1.6em;
	}

	a {
		color: #06c;
		text-decoration: none;

		&:hover {
			text-decoration: underline;
		}
	}
</style>
