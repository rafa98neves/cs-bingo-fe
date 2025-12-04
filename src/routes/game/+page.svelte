<script lang="ts">
	import Card from '../../components/molecules/Card.svelte';
	import Timer from '../../components/molecules/Timer.svelte';
	import type { GameItem, GamePlayer } from '../../types/interfaces/game';
	import { gameData } from './game.data';

	let currentPlayer: GamePlayer = gameData.players.pop()!;

	let timerComp: { reset: () => void }; // seconds

	const options = gameData.remit.slice(0, 4 * 4);

	const responses: Record<number, GameItem> = {};

	const newRound = () => {
		currentPlayer = gameData.players.pop()!;
		timerComp?.reset();
	};

	const onOptionClick = (player: GameItem) => {
		responses[currentPlayer.id] = player;
		newRound();
	};
</script>

<svelte:head>
	<title>Game</title>
	<meta name="description" content="CS Bingo game to play with friends" />
</svelte:head>

<section>
	<h1>Game Page</h1>
	<p>This is where the game will be played.</p>

	<Card class="p-0!">
		<div
			class="gap-4 justify-items-between py-2 bg-slate-200 px-4 grid grid-cols-3 items-center border-b-1"
		>
			<Timer class="basis-24" bind:this={timerComp} finish={newRound} />
			<h3>{currentPlayer.name}</h3>
			<div class="text-right">
				<div>Skip</div>
				<p>{gameData.players.length} remaining</p>
			</div>
		</div>

		<div class="grid grid-cols-4 grid-rows-4">
			{#each options as option}
				<Card clickable onclick={() => onOptionClick(option)} rounded={false} class="border-0!">
					<div class="text-sm flex h-full flex-col items-center justify-center">
						{#if option.prefix}
							<span class="mb-1 block italic">{option.prefix}</span>
						{/if}
						<span class="text-xl font-bold align-middle">{option.displayName}</span>
						{#if option.suffix}
							<span class="mt-1 block italic">{option.suffix}</span>
						{/if}
					</div>
				</Card>
			{/each}
		</div>
	</Card>
</section>

<style lang="scss" scoped>
</style>
