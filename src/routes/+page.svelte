<style>
    .letter-box {
        height: 5px;
        width: 5px;
        border: 2px solid #3a3a3c;
        padding: 15px;
        margin: 3px;
        color: black;
        display: inline-flex;
        justify-content: center;
        align-items: center;
    }

    .row {
        display: flex;
        flex-direction: row;
    }

    .letter-box-wrapper {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-grow: 1;
        overflow: hidden;
        padding: 10px;
        flex-direction: column;
    }

    .col {
        display: flex;
        flex-direction: column;
    }

    .app-container {
        width: 100%;
        max-width: 500px;
        margin: 0 auto;
        display: flex;
        flex-direction: column;
    }
    .key-row {
        display: flex;
        width: 100%;
        margin: 0 auto 8px;
    }
    .key {
        font-size: 1.25em;
        font-weight: bold;
        border: 0;
        padding: 0;
        margin: 0 6px 0 0;
        height: 58px;
        border-radius: 4px;
        cursor: pointer;
        background-color: #818384;
        color: #ffffff;
        flex: 1;
        display: flex;
        justify-content: center;
        align-items: center;
        text-transform: uppercase;
    }

    .key-spacer {
        flex: .5;
    }
</style>
<script>
	import wordleList from '../data';
	let errorMessage = '';
	const ENTER_KEY = '⏎';
	const BACK_SPACE_KEY = '⌫';
	const keyboard = [
		['Q','W','E','R','T', 'Y','U','I', 'O','P'],
		['A','S','D','F','G', 'H','J','K', 'L'],
		[ENTER_KEY,'Z','X','C','V','B','N','M', BACK_SPACE_KEY],
	];
	const enteredLetters = [
		['','','','',''],['','','','',''],['','','','',''],['','','','',''],['','','','','']
	];

	let completedLetterRows = 0;
	let currentLetterIndex = 0;
	const onLetterClick = (letter) => {
		if (letter === ENTER_KEY) {
			const thisWord = enteredLetters[completedLetterRows].join('');
			debugger;
			if (thisWord.length < enteredLetters[0].length - 1) {
				errorMessage = 'Not enough letters';
				return;
			}
			if (wordleList.includes(thisWord)) {
				currentLetterIndex = 0;
				completedLetterRows++;
			}
			errorMessage = 'Invalid word';
			return;
		}

		if (currentLetterIndex <= enteredLetters[0].length - 1) {
			enteredLetters[completedLetterRows][currentLetterIndex] = letter;
			currentLetterIndex ++;
		}
		console.debug(JSON.stringify(enteredLetters));
	}
</script>

<h1>WORDLE</h1>
<div class='app-container'>
	<div class='letter-box-wrapper '>
		{#each enteredLetters as letters, rowIndex}
			<div class='row'>
				{#each letters as letter, letterIndex}
					<div class='letter-box col'>{letter}</div>
				{/each}
			</div>
		{/each}
	</div>
	<div>{errorMessage}</div>
	<div class='keyboard'>
		{#each keyboard as row, rowIndex}
			<div class='key-row'>
				{#if rowIndex === 1}
					<div class='key-spacer'/>
				{/if}
				{#each row as letter, colIndex}
					<button class='key' on:click={() => onLetterClick(letter)}>{letter}</button>
				{/each}
				{#if rowIndex === 1}
					<div class='key-spacer'/>
				{/if}
			</div>
		{/each}

	</div>
</div>
