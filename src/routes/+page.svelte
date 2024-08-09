<script>
	import { onMount } from 'svelte';
	import { browser } from '$app/environment';

	const numCells = 1568;
	const sectionLength = 532;
	const letters = ['A', 'A', 'A', 'A', 'A', 'B', 'C', 'D', 'E', 'F'];
	const numbers = ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0'];
	const chars = ['[', ']', '.', ';', ':', '<', '>', '*', '^', '|', ')', '(', '{', '}', '$', '#', '@', '\\', '/', '!', '?', '4', '7', '8', '6', '5', '~', '%', '-', '_', '¨'];
	const wordLenghts = [5, 6, 7, 8];

	const getNumber = () => numbers[Math.floor(Math.random() * numbers.length)];
	const getLetter = () => letters[Math.floor(Math.random() * letters.length)];
	const getChar = () => chars[Math.floor(Math.random() * chars.length)];
	const getWordLenght = () => wordLenghts[Math.floor(Math.random() * wordLenghts.length)];
	const sectionLeft = [7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25];
	const sectionRight = [35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53];

	const fetchWords = async (length) => {
		const response = await fetch('https://random-word-api.herokuapp.com/word?number=300'); // Example API
		const words = await response.json();
		return words.filter((word) => word.length == length);
	};

	const generateSection = (words) => {
		let cells = Array(sectionLength);
		for (let i = 0; i < cells.length; i++) {
			cells[i] = getChar();
		}
		let currentPosition = 0;

		for (const word of words) {
			if (currentPosition >= sectionLength) break;

			const startPosition = currentPosition;
			const wordLength = word.length;

			// Ensure the word fits in the remaining space
			if (startPosition + wordLength >= sectionLength) break;

			// Place the word in the grid
			for (let i = 0; i < wordLength; i++) {
				cells[startPosition + i] = word[i].toUpperCase();
			}

			// Move to the next position, leaving some space
			currentPosition = startPosition + wordLength + Math.floor(Math.random() * 15) + 8; // Random space between 1 and 5 cells
		}

		return cells;
	};

	onMount(async () => {
		if (!browser) return;

		const wordLength = getWordLenght();
		const wordsLeft = await fetchWords(wordLength);
		const wordsRight = await fetchWords(wordLength);

		console.log(wordsLeft);
		console.log(wordsRight);

		const leftSection = generateSection(wordsLeft);
		const rightSection = generateSection(wordsRight);

		console.log(leftSection);

		const frame = document.getElementById('frame');

		for (let i = 0; i < numCells; i++) {
			let div = document.createElement('div');
			div.className = 'cell';

			switch (i % 28) {
				case 0:
					div.innerHTML = '0';
					break;
				case 1:
					div.innerHTML = 'x';
					break;
				case 4:
					div.innerHTML = getLetter();
					break;
				case 2:
				case 3:
				case 5:
					div.innerHTML = getNumber();
					break;
				case 6:
					div.innerHTML = '-';
					break;
				case 27:
				case 26:
					div.innerHTML = '';
					break;
				default:
					div.innerHTML = getChar();
			}

			frame.appendChild(div);
		}
		for (let i = 0, j = -1, k = -1; i < numCells; i++) {
			const isLeft = sectionLeft.some((val) => val == i % 28);
			const isRight = sectionRight.some((val) => val == i % 56);
			// 532
			if (isLeft && !isRight) {
				j++;
				frame.childNodes[i].innerHTML = leftSection[j];
			} else if (isRight) {
				k++;
				frame.childNodes[i].innerHTML = rightSection[k];
			} else continue;
		}
	});
</script>

<!-- DUMMY -->
<div style="display: none;" class="cell"></div>

<div id="wrap">
	<!--<a href="/Matas-gavekort.pdf" target="_blank" download>Download</a>-->
	<div id="frame"></div>
</div>

<!-- 2240 cells -->

<style lang="scss">
	#wrap {
		height: 100vh;
		width: 100vw;
		background-image: url('/bg.jpg');
		background-size: cover;
		background-repeat: no-repeat;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 20px;
	}
	#frame {
		width: 68%;
		height: 75%;
		display: grid;
		grid-template-columns: repeat(56, 1fr);
		grid-template-rows: repeat(28, 1fr);
	}
	:global(.cell) {
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 1rem;
		color: $primary;
	}
</style>
