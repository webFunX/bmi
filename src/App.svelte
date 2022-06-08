<!-- 
	Wygld skopiowany z tego Kalkulatora BMI:
	https://www.ikard.pl/badanie-bmi.html
-->
<script>
	const tArr = [
    ["poni|ej 16"," - wygBodzenie", 16], // mniej ni| 16
    ["16 - 16.99"," - wychudzenie", 17], // mniej ni| 17
    ["17 - 18.49"," - niedowag", 18.5], // ...
    ["18.5 - 24.99"," - wag prawidBow", 25],
    ["25.0 - 29.9"," - nadwag", 30],
    ["30.0 - 34.99"," - I stopieD otyBo[ci", 35],
    ["35.0 - 39.99"," - II stopieD otyBo[ci", 40],
    ["od 40.0"," - otyBo[ skrajn", Infinity]
	];
	const bmi = (waga,wzrost)=> Math.round((waga/((wzrost/100)**2) + Number.EPSILON) * 100) / 100;
	
	let waga = null;
	let wzrost = new URLSearchParams(location.search).get("wzrost")||null;
	let bmiRes = null;
	let normaOd = null;
	let normaDo = null;
// 	$:bmiRes = bmi(waga,wzrost);
	let obecna = false;
	let wzrost2 = null;
	
	const czysc = _=>{
		waga = null;
		wzrost = null;
		bmiRes = null;
		wzrost2 = null;
	}
	const licz = _=>{
		if (waga>0 && wzrost>0) {
			bmiRes = bmi(waga,wzrost);
			normaOd = Math.round((
						(tArr[3-1]?tArr[3-1][2]:0)*((wzrost/100)**2)
					) * 10) / 10;
			normaDo = Math.round((
				(tArr[4-1]?tArr[4-1][2]:0)*((wzrost/100)**2)  - 0.1
			) * 10) / 10;
			wzrost2 = wzrost;
		} else {
			bmiRes = null;
		}
		// zaznacz na li[cie
		
		// wypisz prawidBowe granica wagi dla tego wzrostu
		
		// wpisz twój bmi, do elementu listy
		// obok przycisku oblicz te| mo|e by
		// i ile brakuje do nastpnej/poprzedniej granicy
	}
</script>
<h2>Badanie BMI</h2>
<div class="con">
	<div class="left">
		<h5><strong>Oblicz swoje BMI</strong></h5>
		<div style="padding-left: 10px;">
		<p class="p">
		<input required min=1 step=1 bind:value={waga} placeholder="Waga"> <strong>kg</strong>
		</p>
		<p class="p">
		<input required min=1 step=1 bind:value={wzrost} placeholder="Wzrost"> <strong>cm</strong></p>
		<button on:click={licz} style="margin:0px 0px 0px 0px;" disabled={!(waga>0 && wzrost>0)}>Oblicz</button>
		<button on:click={czysc} style="margin:0px 0px 0px 0px;" disabled={!(waga!==null || wzrost!==null || bmiRes!==null)}>Wyczy[</button>
			<h5 hidden={bmiRes===null}><strong>Twoje BMI wynosi: </strong>{bmiRes}</h5>
			<h5 hidden={bmiRes===null}><strong>PrawidBowa waga:</strong><br>
			{normaOd}	- {normaDo} kg
			</h5>
	</div>
		
	</div>
	<div class="center">
	</div>
	<div class="right">
		<h5>Dla osób dorosBych<wbr> warto[ BMI wskazuje na:</h5>
		<ul>
			{#each tArr as l, i}
			<li class:obecna={
				bmiRes!==null
				&&
				bmiRes<l[2]
				&&
				bmiRes>=(tArr[i-1]?tArr[i-1][2]:0)
			} class="l">
				<strong>{l[0]}</strong><wbr>
				{l[1]}<wbr>
				<span hidden={bmiRes===null}>- od {
					Math.round((
						(tArr[i-1]?tArr[i-1][2]:0)*((wzrost2/100)**2)
					) * 10) / 10
					
					} kg</span>
			</li>
			{/each}
		</ul>
	</div>
</div>

<p style="text-align: center;"><a style="color: #777; text-decoration: none;" href="https://bmi22.pl">BMI</a> = <strong>waga</strong><sub>(kg)</sub> / <strong>wzrost</strong><sup><strong>2</strong></sup><sub>(m)</sub></p>

<style>
	.con {
		display: flex;
		justify-content: space-between;
		gap: 5px;
		flex-wrap: wrap;
		
		border-radius: 15px;
    border: 2px solid #C91217;
    padding: 20px;
    margin-left: 10px;
    margin-bottom: 20px;
		overflow: auto;
	}
	.left {
		flex-shrink: 0;
	}
	.right {
		min-width: 70px;
	}
	strong {
    font-weight: 700;
	}
	h2 {
		margin-bottom: 10px;
    margin-top: 15px;
		font-family: 'PT Sans', sans-serif;
    font-weight: 700;
    line-height: 1.42857143;
    color: #111111;
	}
	h5 {
		font-weight: 700;
		font-family: 'PT Sans', sans-serif;
		line-height: 1.42857143;
		color: #111111;
		font-size: 16px;
		margin-block-start: 1.67em;
		margin-block-end: 1.67em;    	
		margin-top: 10px;
    margin-bottom: 10px;
	}
	.p {
		color: #C91217 !important;
		margin-bottom: 10.5px;
		line-height: 1.7;
		font-family: 'PT Sans', sans-serif;
		box-sizing: border-box;
	}
		/* Chrome, Safari, Edge, Opera */
/* 		input[type=number]::-webkit-outer-spin-button,
		input::-webkit-inner-spin-button {
			-webkit-appearance: none;
			margin: 0;
		} */

		/* Firefox */
/* 		input[type=number] {
			-moz-appearance: textfield;
		} */
	input {
		border-radius: 5px;
    border: 2px solid #C91217;
    text-indent: 20px;
		font-family: inherit;
    font-size: inherit;
    line-height: inherit;
		margin: 0;
    font: inherit;
		color: inherit;
		max-width: 210px;
		min-width: 60px;
		background-color: var(--bg-color);
	}
	input:focus {
		color: #404040;
		border-color: rgba(0, 0, 0, 0.3);
		-webkit-box-shadow: 0 0 3px rgba(0, 0, 0, 0.1);
		-moz-box-shadow: 0 0 3px rgba(0, 0, 0, 0.1);
		box-shadow: 0 0 3px rgba(0, 0, 0, 0.1);
		outline: none;
	}
	input::placeholder {
		
	}
	ul {
		padding-left: 10px;
		list-style: none;
		margin: 0;
	}
	@media (prefers-color-scheme: dark) {
		:root {
			--bg-color: black;
			--color: silver;
		}
	}
	@media (prefers-color-scheme: light) {
		:root {
			--bg-color: white;
			--color: #777777;
		}
	}
	:global(body) {
		line-height: 1.7;
		background-color: var(--bg-color);
    color: var(--color);
    font-size: 15px;
    font-family: 'PT Sans', sans-serif;
    font-weight: 400;
	}
	button {
		display: inline-block;
		padding: 6px 12px;
		margin-bottom: 0;
		font-size: 14px;
		font-weight: 400;
		line-height: 1.42857143;
		text-align: center;
		white-space: nowrap;
		vertical-align: middle;
		-ms-touch-action: manipulation;
		touch-action: manipulation;
		cursor: pointer;
		-webkit-user-select: none;
		-moz-user-select: none;
		-ms-user-select: none;
		user-select: none;
		background-image: none;
		border: 1px solid transparent;
		border-radius: 4px;
		color: #fff;
    background-color: #C91217;
    border-color: #C91217;
		font-size: 14px;
    padding: 8px 22px;
    line-height: 1.38;
	}
	button:hover {
    color: #fff;
    background-color: #AF0F15;
    border-color: #b90e2b;
		font-size: 14px;
    padding: 8px 22px;
    line-height: 1.38;
	}
	button[disabled] {
		background-color: #D9888B;
		cursor: default;
		border-color: #DB9D9E;
	}
	@media (prefers-color-scheme: dark) {
		h2, h5, p {
			color: white;
		}
		input::placeholder {
			color: red;
			opacity: .75;
		}
		input {
			color: red;
			opacity: 1;
		}
		input:focus, input:focus::placeholder {
			border-color: white;
			color: white;
		}
		button[disabled] {
			background-color: #6A4A4B;
    	border-color: #71595A;
		}
	}
	.obecna {
		 color: rgb(201, 18, 23)
	}

</style>
