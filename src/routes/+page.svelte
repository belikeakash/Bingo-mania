<script>
	import { onMount } from "svelte";
	// @ts-ignore
	import { fly, scale } from 'svelte/transition';
	import { cubicIn } from 'svelte/easing';
	// @ts-ignore
	// @ts-ignore
	let remainingTime = 225;
	let minutes='00';
	let seconds='00';
	function formatTime() {
		var mind = remainingTime % (60 * 60);
		// @ts-ignore
		minutes = Math.floor(mind / 60);
		// @ts-ignore
		if(minutes<10) {
			minutes = '0' + minutes.toString()
		}
		var secd = mind % 60;
		// @ts-ignore
		seconds = Math.ceil(secd);
		// @ts-ignore
		if(seconds<10) {
			seconds = '0' + seconds.toString();
		}
		remainingTime--;
	}
	let markline = false;
	// @ts-ignore
	
	let winning = 0;
	let currWinning = '';
	/**
	 * @type {(string | number)[]}
	 */
	let balls = [];
	function generateBalls() {
		
		// @ts-ignore
		let a2 = [];
		while(balls.length<75) {
			let c = Math.floor(Math.random() * 75) + 1;
			

			let char = ''
				//c = Number(c);
				if(c<=15) {
					char = 'b' 
				}
				else if(c>=16 && c<=30) {
					char = 'i'
				}
				else if(c>=31 && c<=45) {
					char = 'n'
				}
				else if(c>=46 && c<=60) {
					char = 'g'
				}
				else {
					char = 'o'
				}
			// @ts-ignore
			if(!a2.includes(c)) {
				// @ts-ignore
				balls.push({number:c.toString(), character: char});
				a2.push(c);
			}
		}
		console.log(balls);
		console.log(a2);
	}

	onMount(()=>{
		createBoard(); 
		generateBalls(); 
	})
	// @ts-ignore
	/**
	 * @type {any[]}
	 */
	let finalBalls=[];
	// @ts-ignore
	function setBalls(i) {
		finalBalls = balls.slice(i,i+6);
		console.log(finalBalls); 
	}
	let bingoVariable = '';
	function checkComplete() {
		for(let i=0;i<5;i++) {
			if(board[i][0]===board[i][1] && board[i][1]===board[i][2] && board[i][2]===board[i][3] && board[i][3]===board[i][4] && board[i][0]==='⭐️') {
				// @ts-ignore
				bingoVariable = true;
			}
			if(board[0][i]===board[1][i] && board[1][i]===board[2][i] && board[2][i]===board[3][i] && board[3][i]===board[4][i] && board[0][i]==='⭐️') {
				// @ts-ignore
				bingoVariable = true;
			}
		}
		if(board[0][0]===board[1][1] && board[1][1]===board[2][2] && board[2][2]===board[3][3] && board[3][3]===board[4][4] && board[0][0]==='⭐️') {
			// @ts-ignore
			bingoVariable = true;
		}
		if(board[0][4]===board[1][3] && board[1][3]===board[2][2] && board[2][2]===board[3][1] && board[3][1]===board[4][0] && board[2][2]==='⭐️') {
			// @ts-ignore
			bingoVariable = true;
		}
	}
	let board = [['1','16','31','46','61'],['2','17','32','47','62'],['3','18','x','48','63'],['4','19','34','49','64'],['5','20','35','50','65']];
	/**
	 * @param {number} num
	 */
	function generateBoard(num) {
		/**
		 * @type {number[]} 
		 */
		let arr =[]; 

		let diff = 15;
		while(arr.length<5) {
			let u = Math.floor(Math.random()*diff) + 1;
			u = u + num
			// @ts-ignore
			if(!arr.includes(u)) {
				arr.push(u);
			}
		}
		// for(let i=0;i<5;i++) {
		// 	let u = Math.floor(Math.random()*diff) + 1;
		// 	u = u + num
		// 	arr.push(u);
		// }
		//console.log(arr)
		return arr;
	} 
	function createBoard() {
		// @ts-ignore
		// @ts-ignore
		// @ts-ignore
		let j = 0;
		
		for(let i=0;i<5;i++) {
			let a1 = generateBoard(i*15);
			//console.log(a1);
			
			for(let k=0;k<5;k++) {
				let c = a1[k];
				board[k][i] =c.toString();
			}
			
		}
		board[2][2] = '⭐️'
	}
	let text = 'Start Game'
	function check() {
		checkComplete();
		if(bingoVariable==='') {
			// @ts-ignore
			bingoVariable = false;
			text = 'BINGO';
			setBalls(0);
			// @ts-ignore
			number = finalBalls[0].number;
			startGame();
			formatTime();
			setInterval(()=>{
				console.log();
				formatTime();
			},1000)
		}
		else {
			// @ts-ignore
			if(bingoVariable===false) {
				// @ts-ignore
				currWinning = 1400;
				winning-=1400;
			}
			else {
				markline = true;
				currWinning+=4200;
				winning+=4200;
			}
		}
	}
	// @ts-ignore
	// @ts-ignore
	let imgNode = `<img src='/cross.png' alt='alt' class='h-10 w-10' />`
	let number = '';
	let t1='';
	function startGame() {
		// @ts-ignore
		let i=7;
		if(t1!=='') return;
		// @ts-ignore
		t1 = setInterval(()=>{
			// @ts-ignore
			i++;
			// @ts-ignore
			let a3 = finalBalls;
			let a2 = finalBalls;
			//a2.splice(0,1);
			
			for(let k=0;k<a3.length-1;k++) {
				a3[k] = a3[k+1];
			}
			console.log(finalBalls);
			finalBalls = [...a3];
			//finalBalls = [...a2];
			if(i<75) {
				a3[a3.length-1] = balls[i];
				finalBalls = [...a3];
				//finalBalls=[...finalBalls, balls[i]];
			}
			if(i>=80) {
			stopGame();
			}
			number = finalBalls[0]?.number;
			//prog.set($prog+1);
			
			console.log(finalBalls);
		}, 3000)
		
	}
	function stopGame() {
		// @ts-ignore
		clearInterval(t1)
	}
	// @ts-ignore
	function checkNumber(n1, n2) {
		let int;
		if(bingoVariable==='') return;
		if(board[n1][n2]==='❌') return;
		else if(board[n1][n2]!==number) {winning-=1400; board[n1][n2] = '❌'; 
		// @ts-ignore
		currWinning = 1400;
		if(int) {
			clearInterval(int);
		}
		int = setInterval(()=>{
			currWinning=''
		},1200)
	}
		else if(board[n1][n2]===number) {winning+=4300; board[n1][n2] = '⭐️'
		// @ts-ignore
		int = currWinning = 4300;
		if(int) {
			clearInterval(int);
		}
		int = setInterval(()=>{
			currWinning=''
		},1200)
	}
		console.log(board[n1][n2]);
	}
	
</script>
<div class="w-[360px] bg-[#471678] h-full">
<div class="p-2 bg-[#471678]">
	<div class="flex h-fit justify-between items-center">
		<img src="/bingo.png" class="h-[50px] w-[161px]" alt="bingo-icon" />
		<div class="flex h-fit align-middle relative w-[82px] items-center">
			<div
				class="z-50 bg-yellow-300 h-7 w-8 flex align-middle justify-center rounded-full text-red-700"
			>
				x 
			</div>
			<div
				class="text-white items-center w-[60px] z-0 relative -left-2 align-middle flex justify-center text-xs border-2 rounded-[30px] border-red-500 h-6 leading-3"
			>
				Close
			</div>
		</div>
	</div>
	<div class="pt-2 flex w-auto ">
		<div class="flex">
		<div class="flex justify-center items-center">
			<div class="z-20 text-3xl text-yellow-500">⏳</div>
			<div class="text-white relative  text-sm -left-2 pl-2 h-6 flex justify-center items-center border rounded-3xl p-1 border-orange-300 pr-4 pl-4">{minutes}: {seconds}</div>
		</div>
		<div class="flex justify-center items-center">
			<div class="z-20 text-xl text-yellow-400">🪙</div>
			<div class="text-white relative -left-2 pl-2 text-sm h-6 flex justify-center items-center border rounded-3xl p-1 border-orange-300 pr-4 pl-4">{winning}</div>
		</div>
	</div>
	<div class='ml-3 text-red-500 flex items-center'>{currWinning}</div>
	</div>
</div>
<div class="p-2 pr-0 h-[130px] bg-[#2a074e]">
	<div class="flex justify-center text-white">
		Bingo Numbers
	</div>
	<div class="gap-2 flex overflow-hidden items-center">
		<!-- balls -->
		{#each finalBalls as balls, idx(balls)}
		{#if idx===0} 
		<div in:fly={{ x: 200, duration: 100, easing:cubicIn }}  out:fly={{ x: -40, duration: 100, easing:cubicIn }} class="shrink-0 h-20 w-20 rounded-full flex bg-yellow-300 justify-center items-center">
			<div class="h-12 w-12 rounded-full bg-white flex flex-col justify-center align-middle items-center">
				<div class="">{balls.number}</div>
				<div class="">{balls.character}</div>
			</div>
		</div>
		{:else if idx===1} 
		<div in:fly={{ x: 0, duration: 100, easing:cubicIn }} out:fly={{ x: -40, duration: 100, easing: cubicIn }} class="shrink-0 h-16 w-16 rounded-full flex bg-red-500 justify-center items-center">
			<div  class="h-12 w-12 rounded-full bg-white flex flex-col justify-center align-middle items-center">
				<div class="">{balls.number}</div>
				<div class="">{balls.character}</div>
			</div>
		</div>
		{:else if idx===2} 
		<div in:fly={{ x: 0, duration: 100, easing:cubicIn }} out:fly={{ x: -40, duration: 100, easing: cubicIn }} class="shrink-0 h-16 w-16 rounded-full flex bg-blue-300 justify-center items-center">
			<div  class="h-12 w-12 rounded-full bg-white flex flex-col justify-center align-middle items-center">
				<div class="">{balls.number}</div>
				<div class="">{balls.character}</div>
			</div>
		</div>
		{:else if idx===3} 
		<div in:fly={{ x: 0, duration: 100, easing:cubicIn }} on:introstart={()=>console.log('intro start')} out:fly={{ x: -40, duration: 100, easing: cubicIn }} class="shrink-0 h-16 w-16 rounded-full flex bg-green-300 justify-center items-center">
			<div  class="h-12 w-12 rounded-full bg-white flex flex-col justify-center align-middle items-center">
				<div class="">{balls.number}</div>
				<div class="">{balls.character}</div>
			</div>
		</div>
		{:else if idx===4} 
		<div in:fly={{ x: 0, duration: 100, easing:cubicIn }} out:fly={{ x: 0, duration: 100, easing: cubicIn }} class="shrink-0 h-16 w-16 rounded-full flex bg-pink-300 justify-center items-center">
			<div  class="h-12 w-12 rounded-full bg-white flex flex-col justify-center align-middle items-center">
				<div class="">{balls.number}</div>
				<div class="">{balls.character}</div>
			</div>
		</div>
		{:else} 
		<div in:fly={{ x: 0, duration: 100, easing:cubicIn }} out:fly={{ x: 0, duration: 100, easing: cubicIn }} class="shrink-0 h-16 w-16 rounded-full flex bg-yellow-300 justify-center items-center">
			<div  class="h-12 w-12 rounded-full bg-white flex flex-col justify-center align-middle items-center">
				<div class="">{balls.number}</div>
				<div class="">{balls.character}</div>
			</div>
		</div>
		{/if}
		{/each}
		
	</div>
</div>
<div class='m-auto '>
	
</div>
<div class="" >
{#if markline}	
<div class="flex mx-auto relative top-9 h-2 w-80 bg-slate-500" />
{/if}
	<div class="gap-1 flex justify-center items-center mt-2 -mb-3">
		<div class="h-13 w-13 items-center text-white text-2xl flex justify-center"><img src="/b.png" alt="" /> </div>
		<div class="h-13 w-13 items-center text-white text-2xl flex justify-center"><img src="/i.png" alt="" /> </div>
		<div class="h-13 w-13 items-center text-white text-2xl flex justify-center"><img src="/n.png" alt="" /> </div>
		<div class="h-13 w-13 items-center text-white text-2xl flex justify-center"><img src="/g.png" alt="" /> </div>
		<div class="h-13 w-13 items-center text-white text-2xl flex justify-center"><img src="/o.png" alt="" /> </div>
	</div>
</div>
<div class="flex justify-center ">
	
<div class="">
<div class="p-2 bg-red-400 flex justify-center items-center rounded-xl">
	<div class=" p-2 bg-yellow-400 flex justify-center items-center rounded-xl">
		<div class=" space-y-2 p-2 bg-yellow-200  justify-center items-center rounded-xl">
			{#each board as e1, idx1} 
		<div class="flex gap-2 ">
		
		{#each e1 as e2, idx2} 
			{#if e2[0]==='<'}
			<div class=' text-xl rounded-lg shadow-xl border h-12 w-12 flex items-center justify-center bg-white' on:click={(e)=>checkNumber(idx1, idx2)}>{@html e2}</div>
			{:else}
			<div class=' text-xl rounded-lg shadow-xl border h-12 w-12 flex items-center justify-center bg-white' on:click={(e)=>checkNumber(idx1, idx2)}>{e2}</div>
			{/if}
			
		{/each}
	</div>
	{/each}
	<div class="h-2" />
	<div on:click={check} class=" text-white bg-green-600 p-2 flex justify-center text-xl rounded-xl mt-3 gap-0 cursor-pointer">
		{text}
	</div>
		</div>
	</div>
</div>

</div>
</div>

</div>