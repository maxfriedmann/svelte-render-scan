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

	// Derived visual effects
	let nameRepeated = $derived(name.repeat(Math.floor(happiness / 20)));
	let ageSquares = $derived(Array.from({ length: age }, (_, i) => i));
	let happinessEmoji = $derived.by(() => {
		if (happiness < 20) return '😢';
		if (happiness < 40) return '😕';
		if (happiness < 60) return '😐';
		if (happiness < 80) return '😊';
		return '😁';
	});
	let skillSymbols = $derived.by(() => {
		const symbols = {
			beginner: '🌱',
			intermediate: '🌿',
			advanced: '🌳',
			expert: '🎓'
		};
		return symbols[programmingLevel as keyof typeof symbols] || '🌱';
	});
	let notificationCount = $derived(Object.values(notifications).filter(Boolean).length);

	// Derived summary for display
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
			<div class="visual-feedback" style="font-size: {Math.min(happiness / 2 + 50, 150)}%">
				{nameRepeated || 'Type your name...'}
			</div>
		</div>

		<!-- Number input -->
		<div class="form-group">
			<label for="age">Age:</label>
			<input type="number" id="age" bind:value={age} min="0" max="150" />
			<div class="visual-feedback squares">
				{#each ageSquares as square}
					<div
						class="square"
						style="background-color: {favoriteColor}; transform: rotate({happiness}deg);"
					/>
				{/each}
			</div>
		</div>

		<!-- Color picker -->
		<div class="form-group">
			<label for="color">Favorite Color:</label>
			<input type="color" id="color" bind:value={favoriteColor} />
			<div class="visual-feedback" style="color: {favoriteColor}; font-size: 2rem;">
				{#each Array(3) as _}
					<span style="opacity: {happiness / 100}">★</span>
				{/each}
			</div>
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
			<div class="visual-feedback">
				<span style="font-size: 2rem;">{skillSymbols}</span>
				{#each Array(skillLevels.findIndex((l) => l.value === programmingLevel) + 1) as _}
					<span class="growth-indicator">↗️</span>
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
			<div class="visual-feedback">
				{#each Array(notificationCount) as _}
					<span class="notification-indicator">🔔</span>
				{/each}
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
			<div class="visual-feedback interests">
				{#each interests as interest}
					<span
						class="interest-tag"
						style="background-color: {favoriteColor}; opacity: {happiness / 100};"
					>
						{interest}
					</span>
				{/each}
			</div>
		</div>

		<!-- Range slider -->
		<div class="form-group">
			<label for="happiness">Happiness Level: {happiness}% {happinessEmoji}</label>
			<input type="range" id="happiness" bind:value={happiness} min="0" max="100" step="1" />
			<div class="visual-feedback">
				<div
					class="happiness-bar"
					style="width: {happiness}%; background-color: {favoriteColor};"
				/>
			</div>
		</div>

		<!-- Textarea -->
		<div class="form-group">
			<label for="bio">Bio:</label>
			<textarea id="bio" bind:value={bio} rows="4" placeholder="Tell us about yourself" />
			<div class="visual-feedback bio-stats">
				<span>Characters: {bio.length}</span>
				<span>Words: {bio.trim() ? bio.trim().split(/\s+/).length : 0}</span>
			</div>
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
		margin-bottom: 2.5rem;
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

	.visual-feedback {
		margin-top: 0.5rem;
		min-height: 2rem;
	}

	.squares {
		display: flex;
		flex-wrap: wrap;
		gap: 4px;
	}

	.square {
		width: 10px;
		height: 10px;
		transition: transform 0.3s ease;
	}

	.happiness-bar {
		height: 20px;
		background-color: #ff3e00;
		border-radius: 10px;
		transition: width 0.3s ease;
	}

	.interest-tag {
		display: inline-block;
		padding: 0.25rem 0.5rem;
		border-radius: 15px;
		margin: 0.25rem;
		color: white;
		font-size: 0.875rem;
	}

	.notification-indicator {
		font-size: 1.5rem;
		margin-right: 0.5rem;
	}

	.growth-indicator {
		display: inline-block;
		margin-left: 0.25rem;
	}

	.bio-stats {
		display: flex;
		gap: 1rem;
		color: #666;
		font-size: 0.875rem;
	}
</style>
