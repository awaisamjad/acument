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
	@import url('https://fonts.googleapis.com/css2?family=Merriweather:wght@300;400;700&display=swap');

// Variables
$primary-color: #4d6e8f;
$secondary-color: #f2f2f2;
$text-color: #333;
$link-color: #6699cc;
$link-hover-color: #336699;

// Base Styles
body {
  font-family: 'Merriweather', serif;
  color: $text-color;
  line-height: 1.6;
  background-color: $secondary-color;
}

// Headings
h1, h2, h3, h4, h5, h6 {
  color: $primary-color;
  font-weight: 700;
  margin-bottom: 0.8em;
  line-height: 1.4;
}

// Paragraphs
p {
  line-height: 1.8;
  margin-bottom: 1.2em;
}

// Links
a {
  color: $link-color;
  text-decoration: none;
  transition: color 0.3s ease;

  &:hover,
  &:focus {
    color: $link-hover-color;
    text-decoration: underline;
  }
}

// Layout
.container {
  max-width: 960px;
  margin: 0 auto;
  padding: 2rem;
}

// Buttons
.btn {
  display: inline-block;
  padding: 0.8rem 1.6rem;
  background-color: $primary-color;
  color: #fff;
  text-decoration: none;
  border-radius: 4px;
  transition: background-color 0.3s ease;

  &:hover,
  &:focus {
    background-color: darken($primary-color, 10%);
  }

	}
</style>
