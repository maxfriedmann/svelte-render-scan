<script lang="ts">
	import renderScan from '$lib/render-scan';
	import { onMount } from 'svelte';

	onMount(() => {
		renderScan();
	});
	// Form state
	let name = $state('');
	let age = $state(25);
	let favoriteColor = $state('#ff3e00');
	let programmingLevel = $state('intermediate');
	let notifications = $state({
		email: true,
		sms: false,
		push: true
	});
	let interests = $state<string[]>([]);
	let bio = $state('');
	let happiness = $state(50);

	// Derived values
	let formSummary = $derived({
		name,
		age,
		favoriteColor,
		programmingLevel,
		notifications,
		interests,
		bio,
		happiness
	});

	// Available options
	const skillLevels = [
		{ value: 'beginner', label: 'Beginner' },
		{ value: 'intermediate', label: 'Intermediate' },
		{ value: 'advanced', label: 'Advanced' },
		{ value: 'expert', label: 'Expert' }
	];

	const interestOptions = [
		'Frontend Development',
		'Backend Development',
		'DevOps',
		'Machine Learning',
		'Mobile Development',
		'Game Development'
	];
</script>

<main class="container">
	<h1>Interactive Form Demo</h1>
	<p>Experience fine-grained reactivity with Svelte 5</p>

	<form class="form-container">
		<!-- Text input -->
		<div class="form-group">
			<label for="name">Name:</label>
			<input type="text" id="name" bind:value={name} placeholder="Enter your name" />
		</div>

		<!-- Number input -->
		<div class="form-group">
			<label for="age">Age:</label>
			<input type="number" id="age" bind:value={age} min="0" max="150" />
		</div>

		<!-- Color picker -->
		<div class="form-group">
			<label for="color">Favorite Color:</label>
			<input type="color" id="color" bind:value={favoriteColor} />
		</div>

		<!-- Radio buttons -->
		<div class="form-group">
			<label>Programming Level:</label>
			<div class="radio-group">
				{#each skillLevels as level}
					<label class="radio-label">
						<input type="radio" name="level" value={level.value} bind:group={programmingLevel} />
						{level.label}
					</label>
				{/each}
			</div>
		</div>

		<!-- Checkboxes -->
		<div class="form-group">
			<label>Notification Preferences:</label>
			<div class="checkbox-group">
				<label class="checkbox-label">
					<input type="checkbox" bind:checked={notifications.email} />
					Email
				</label>
				<label class="checkbox-label">
					<input type="checkbox" bind:checked={notifications.sms} />
					SMS
				</label>
				<label class="checkbox-label">
					<input type="checkbox" bind:checked={notifications.push} />
					Push
				</label>
			</div>
		</div>

		<!-- Multiple select -->
		<div class="form-group">
			<label>Interests:</label>
			<div class="checkbox-group">
				{#each interestOptions as interest}
					<label class="checkbox-label">
						<input type="checkbox" value={interest} bind:group={interests} />
						{interest}
					</label>
				{/each}
			</div>
		</div>

		<!-- Range slider -->
		<div class="form-group">
			<label for="happiness">Happiness Level: {happiness}%</label>
			<input type="range" id="happiness" bind:value={happiness} min="0" max="100" step="1" />
		</div>

		<!-- Textarea -->
		<div class="form-group">
			<label for="bio">Bio:</label>
			<textarea id="bio" bind:value={bio} rows="4" placeholder="Tell us about yourself"></textarea>
		</div>
	</form>

	<!-- Live form data display -->
	<div class="form-data">
		<h2>Live Form Data:</h2>
		<pre>{JSON.stringify(formSummary, null, 2)}</pre>
	</div>
</main>

<style>
	.container {
		max-width: 800px;
		margin: 0 auto;
		padding: 2rem;
	}

	h1 {
		color: #ff3e00;
		margin-bottom: 0.5rem;
	}

	.form-container {
		margin-top: 2rem;
	}

	.form-group {
		margin-bottom: 1.5rem;
	}

	label {
		display: block;
		margin-bottom: 0.5rem;
		font-weight: bold;
	}

	input[type='text'],
	input[type='number'],
	textarea {
		width: 100%;
		padding: 0.5rem;
		border: 1px solid #ccc;
		border-radius: 4px;
	}

	.radio-group,
	.checkbox-group {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	.radio-label,
	.checkbox-label {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		font-weight: normal;
	}

	input[type='range'] {
		width: 100%;
	}

	.form-data {
		margin-top: 2rem;
		padding: 1rem;
		background-color: #f9f9f9;
		border-radius: 4px;
	}

	pre {
		white-space: pre-wrap;
		word-wrap: break-word;
	}
</style>
