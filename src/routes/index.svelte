<script lang="ts">
	import { afterUpdate, onMount } from 'svelte';
	let result: string = 'x';
	let hidden: boolean = true;
	function resizeParchment() {
		var content = document.querySelector('#parchment') as HTMLElement;
		var container = document.querySelector('#contain') as HTMLElement;

		// SVG feTurbulence can modify all others elements, that's why "parchment" is in absolute position.
		// so for a better effect, absolute height is defined by his content.
		content.style.height = container.offsetHeight + 'px';
	}
	afterUpdate(() => {
		resizeParchment();
		hidden = false;
	});
	onMount(() => {
		generate();
		resizeParchment();
	});

	const fantasyCreatures = [
		'Dragon',
		'Wizard',
		'Giant',
		'Lizard',
		'Hobbit',
		'Elf',
		'Dwarf',
		'Gnome',
		'Troll',
		'Nymph',
		'Dryad',
		'Satyr',
		'Centaur',
		'Demon',
		'Imp',
		'Orc',
		'Goblin',
		'Werewolve',
		'Hydra'
	];
	const adjectives = [
		'Ruined',
		'Hidden',
		'Lost',
		'Dark',
		'Dawn',
		'Ancient',
		'Frozen',
		'Magic',
		'Celestial',
		'Wicked',
		'Silent',
		'Mighty',
		'Toxic',
		'Iron',
		'Eternal',
		'Sleep',
		'Space',
		'Sacred',
		'Flying',
		'Floating',
		'Doomed',
		'Void',
		'Crimson',
		'Enchanted',
		'Lucid',
		'Desert',
		'Fallen',
		'Lunar',
		'Solar',
		'Dreamy',
		'Haunted',
		'Forgotten',
		'Psychic',
		'Cursed',
		'Orange',
		'Last',
		'Astral',
		'Stoned',
		'Shamanic',
		'Blind',
		'Old',
		'Spirit',
		'Dead',
		'Holy',
		'Lost',
		'Psychedelic',
		'Bloody',
		'Lonely',
		'Dark',
		'Grey',
		'Purple',
		'Smoking',
		'Acid',
		'Esoteric',
		'Secret',
		'Alchemical'
	];
	const places = [
		'Tower',
		'Cathedral',
		'Fortress',
		'Tomb',
		'Valley',
		'Temple',
		'Forest',
		'Dungeon',
		'Hall',
		'Town',
		'Castle',
		'Village',
		'Cave',
		'Crypt',
		'Manor',
		'Lair',
		'Cavern',
		'Wood'
	];
	const nouns = [
		'Staircase',
		'Fountain',
		'Forgetfulness',
		'Glory',
		'Wizard',
		'Lore',
		'Hope',
		'Haze',
		'Sleep',
		'Knight',
		'Lord',
		'Bloom',
		'God',
		'Guild',
		'Darkness',
		'Crystal',
		'Circle',
		'Star',
		'Pharaoh',
		'Queen',
		'Cauldron',
		'Wanderer',
		'Ceremony',
		'Path',
		'Journey',
		'Odyssey',
		'Caravan',
		'Mammoth',
		'Rites',
		'Cult',
		'Beast',
		'Sun',
		'Leaf',
		'Prophet',
		'Priest',
		'Moon',
		'Monolith',
		'Demon',
		'Slumber',
		'Swamp',
		'Druid',
		'Altar',
		'Memories',
		'Rising',
		'Visions',
		'Covenant',
		'Genesis',
		'Sorcerer',
		'Dream',
		'Fuzz',
		'Shadow',
		'Death',
		'Smoke',
		'Curse',
		'Throne',
		'Snake',
		'Serpent',
		'Trip',
		'Void',
		'Meadow',
		'Shaman',
		'Echo',
		'Hallucination',
		'Moth',
		'Tomb',
		'Tale',
		'Spirit',
		'Doom',
		'Spell',
		'Funeral',
		'Children',
		'Ghost',
		'Witch',
		'Gold',
		'Witchcraft',
		'Frost',
		'Blood',
		'Wizard',
		'Goat',
		'Mantis',
		'King',
		'Prince',
		'Warrior',
		'Thief',
		'Mage',
		'Mass',
		'Brotherhood',
		'Throne',
		'Shelter',
		'Golem',
		'Archive',
		'Synth',
		'Gate',
		'Magician',
		'Tome',
		'Scroll',
		...fantasyCreatures,
		...places
	];
	// Source: https://www.reddit.com/r/DungeonSynth/comments/sy7j37/comment/hy08u0v/?utm_source=reddit&utm_medium=web2x&context=3
	//  AND: https://www.reddit.com/r/DungeonSynth/comments/sy7j37/comment/hxwbrhw/?utm_source=reddit&utm_medium=web2x&context=3
	//  AND: https://www.reddit.com/r/DungeonSynth/comments/sy7j37/comment/hxwaj2q/?utm_source=reddit&utm_medium=web2x&context=3

	const commonPatterns = [
		{ values: [adjectives, nouns] },
		{ values: [['The', ''], nouns, ['of the', 'and the'], adjectives, nouns] },
		{ values: [nouns, ['and', 'and', 'and', 'with', 'without', 'for', 'for a', 'or'], nouns] },
		{ values: [adjectives, nouns, ['and', 'of', 'of the'], nouns] },
		{ values: [['The', ''], adjectives, nouns] },
		{ values: [['The', 'A'], fantasyCreatures, ['in a', 'in the'], places] },
		{ values: [['In the', '', 'The'], places, ['of the'], adjectives, nouns] }
	];
	const rarePatterns = [
		{ values: [['The'], nouns, [', the'], nouns, ['and the'], adjectives, nouns] },
		{ values: [['A'], nouns, [', a'], nouns, [', a'], nouns] },
		{ values: [['Through', 'In the', 'Beyond the', 'Under the', 'Into the'], adjectives, places] },
		{ values: [['A'], nouns, ['with', 'without', 'for a'], nouns] },
		{ values: [['The', '', 'Our', 'Your', 'My', 'Her', 'Their', 'One', 'Any'], nouns] },
		{ values: [adjectives, ['and', 'but', 'or'], adjectives] },
		{ values: [adjectives, nouns, ['and', 'of', 'of the'], adjectives, nouns] },
		{ values: [['The', ''], adjectives, ['and'], adjectives, nouns] },
		{ values: [['The', ''], fantasyCreatures, ["'s"], places] },
		{ values: [['The', ''], adjectives, nouns, ['in the'], places, ['of'], nouns] },
		{ values: [['The', 'A', ''], nouns, ['in a', 'in the'], places] },
		{ values: [['The', ''], adjectives, fantasyCreatures, ["'s"], nouns] }
	];

	const patterns = [
		...commonPatterns,
		...commonPatterns,
		...commonPatterns,
		...commonPatterns,
		...rarePatterns
	];

	function pickRandom(array) {
		return array[Math.floor(Math.random() * array.length)];
	}

	function generate() {
		result = pickRandom(patterns)
			.values.map((e) => pickRandom(e))
			.join(' ')
			.replace(/ ,/gi, ',')
			.replace(/ '/gi, "'")
			.replace(/(^a| a)( [aeiou])/gi, '$1n $2');
	}
</script>

<svelte on:resize={resizeParchment} />

<body class="bg-black text-stone-500">
	<div id="main" class="container m-auto">
		<div id="parchment" />
		<div id="contain" class={hidden ? 'invisible' : ''}>
			<h1
				class="font-['Blackcastle'] text-xl lg:text-4xl mx-auto text-center w-full text-stone-800 mt-10"
			>
				Dungeon Synth Name Generator
			</h1>
			<p class="font-['BreatheFire'] text-2xl lg:text-6xl my-10 text-amber-900 text-center w-full">
				{result}
			</p>
			<div class="mx-auto">
				<button
					class="text-stone-800 px-2 py-1 border border-slate-600 bg-stone-300 rounded"
					on:click={generate}>Generate a new one</button
				>
			</div>
			<div class="mt-10 container m-auto text-center text-xs -mb-10">
				<a
					class="hover:underline"
					href="https://github.com/rap2hpoutre/dungeon-synth-name-generator/">Source code</a
				>
				- <a class="hover:underline" href="https://rap2h.bandcamp.com/">rap2h</a> (<a
					class="hover:underline"
					href="https://unlicense.org/">unlicensed</a
				>)
			</div>
		</div>
	</div>

	<svg>
		<filter id="wavy2">
			<feTurbulence x="0" y="0" baseFrequency="0.02" numOctaves="5" seed="1" />
			<feDisplacementMap in="SourceGraphic" scale="20" />
		</filter>
	</svg>
</body>
