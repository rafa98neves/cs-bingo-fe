<script lang="ts">
	import Card from '../../components/molecules/Card.svelte';
	import Timer from '../../components/molecules/Timer.svelte';
	import { Button } from 'flowbite-svelte';
	import { ArrowRightOutline } from 'flowbite-svelte-icons';
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
			class="justify-items-between grid h-22 grid-cols-6 items-center gap-4 border-b-1 bg-slate-300 px-4 py-2"
		>
			<Timer bind:this={timerComp} finish={newRound} />
			<div class="col-span-4">{currentPlayer.name}</div>
			<div class="flex flex-col items-end">
				<div>
					<Button color="alternative" onclick={newRound} class="border-none">
						Skip
						<ArrowRightOutline size="sm" class="me-2 h-4 w-4" />
					</Button>
				</div>
				<div class="text-sm text-slate-400">{gameData.players.length} remaining</div>
			</div>
		</div>

		<div class="grid grid-cols-4 grid-rows-4">
			{#each options as option}
				<Card
					clickable
					onclick={() => onOptionClick(option)}
					rounded={false}
					class="border-0! {!!responses[currentPlayer.id] ? 'active' : ''}"
					disabled={!!responses[currentPlayer.id]}
				>
					<div class="flex h-full flex-col items-center justify-center text-sm">
						{#if option.prefix}
							<span class="mb-1 block italic">{option.prefix}</span>
						{/if}
						<span class="align-middle text-xl font-bold">{option.displayName}</span>
						{#if option.suffix}
							<span class="mt-1 block italic">{option.suffix}</span>
						{/if}
					</div>
				</Card>
			{/each}
		</div>
	</Card>
</section>

<style scoped>
	.active {
		background-color: red !important;
	}
</style>
