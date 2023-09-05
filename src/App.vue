<script setup>
import { ref, computed, onMounted } from 'vue'

const wins = ref(0)
const draws = ref(0)
const losses = ref(0)

const choice = ref(null)
const computerChoice = ref(null)
const verdict = ref(null)

const outcomes = {
	'ค้อน': {
		'ค้อน': 'draw',
		'กระดาษ': 'loss',
		'กรรไกร': 'win'
	},
	'กระดาษ': {
		'ค้อน': 'win',
		'กระดาษ': 'draw',
		'scissors': 'loss'
	},
	'กรรไกร': {
		'ค้อน': 'loss',
		'กระดาษ': 'win',
		'กรรไกร': 'draw'
	}
}

const play = (c) => {
	choice.value = c
	
	const choices = ['ค้อน', 'กระดาษ', 'กรรไกร']
	const random = Math.floor(Math.random() * choices.length)
	computerChoice.value = choices[random]

	const outcome = outcomes[choice.value][computerChoice.value]
	
	if (outcome === 'win') {
		wins.value++
		verdict.value = 'คุณชนะ!'
	} else if (outcome === 'loss') {
		losses.value++
		verdict.value = 'คุณเเพ้!'
	} else {
		draws.value++
  verdict.value = 'เสมอ!'
	}

	SaveGame()
}

const winPercentage = computed(() => {
	const total = wins.value + draws.value + losses.value
	return total ? (wins.value / total) * 100 : 0
})

const SaveGame = () => {
	localStorage.setItem('wins', wins.value)
	localStorage.setItem('draws', draws.value)
	localStorage.setItem('losses', losses.value)
}

const LoadGame = () => {
	wins.value = parseInt(localStorage.getItem('wins')) || 0
	draws.value = parseInt(localStorage.getItem('draws')) || 0
	losses.value = parseInt(localStorage.getItem('losses')) || 0
}

const ResetRound = () => {
	choice.value = null
	computerChoice.value = null
	verdict.value = null
}

onMounted(() => {
	LoadGame()

	window.addEventListener('keypress', (e) => {
		if (e.key === 'r') {
			ResetRound()
		}
	})
})
</script>

<template>
	<div class="bg-gray-400 text-white text-center min-h-screen flex flex-col">
		<header class="container mx-auto p-6">
			<h1 class="text-4xl font-bold">เกมส์เป่า ยิ้ง ฉุบ!</h1>
		</header>

		<main class="container mx-auto p-6 flex-1">
			<div v-if="choice === null" class="flex items-center justify-center -mx-6">

				<button @click="play('ค้อน')"
					class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 shover:bg-red-500">
					<img src="./assets/01.png" alt="ค้อน" class="w-full" />
				</button>

				<button @click="play('กระดาษ')"
					class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-green-500">
					<img src="./assets/03.png" alt="กระดาษ" />
				</button>

				<button @click="play('กรรไกร')"
					class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-blue-500">
					<img src="./assets/02.png" alt="กรรไกร" />
				</button>

			</div>

			<div v-else>
				<div class="text-3xl mb-4">
					คุณ เลือก: <span class="text-pink-500">{{ choice }}</span>
				</div>
				<div class="text-3xl mb-4">
					ศัตรู เลือก: <span class="text-green-500">{{ computerChoice }}</span>
				</div>
				<div class="text-6xl mb-12">
					{{ verdict }}
				</div>

				<button @click="ResetRound" class="bg-pink-500 text-lg py-2 px-4">เริ่มอีกครั้ง</button>
			</div>

			<div class="mt-12 text-3xl mb-4">{{ wins }} : {{ draws }} : {{ losses }}</div>

			<div class="text-lg">อัตราชนะ: {{ Math.round(winPercentage) }}%</div>
		</main>

		<footer class="container mx-auto p-6">
			<a href="http://www.cs.mju.ac.th/home">นาย จิตติพัฒน์ สนม รหัส6504101414</a>
		</footer>
	</div>
</template>

<style></style>