<script setup lang="ts">
import { computed, ref } from 'vue';
import html2canvas from 'html2canvas'

let angle = ref(-120)
let preview = ref(false)
let text = ref(`
In this new era of typing and video games, writing homework has become the new unfavorite. People cringe when asked to submit a written assignment. \\n 

If that sounds like you, this is for you! \\n \n

A new, simple way of "writing" assignments and "scanning" them to your teacher(aka typing your notes and getting this natural, handwriting - like image which looks like a scanned copy). \\n 

Keep in mind that this is an experiment, definately not production - ready. Feel free to join me @GitHub and fix the bugs, make enhancements, and serve a rather important section of the world's population: the students! \\n 

 \\n 
 \\n 
 \\n 
 \\n 
Created with ♥ for students.`);

function random(min: number, max: number) {
	return Math.random() * (max - min) + min
}

const letterStyle = (letter: string) => {
	console.log(letter.toUpperCase() == letter)
	return {
		fontFamily: `${letter.toUpperCase() === letter ? 'Caveat' : 'Cedarville Cursive'}`,
		fontSize: `${random(16, 20)}px`,
		color: '#000F55',
		transform: `skew(${random(-8, 8)}deg,${random(-8, 8)}deg) translateY(${random(-1, 1)}px)`,
		transition: 'all 0.5s ease-in-out',
		display: letter === ' ' ? '' : 'inline-block',
	}
}

const background = computed(() => {
	return {
		background: `linear-gradient(${angle.value}deg, #0008, #0000)`
	}
})
const words = computed(() => {
	return text.value.split(' ')
})
// download image with html2canvas 
const downloadImage = async () => {
	const element = document.querySelector('.page')
	if (!element) return
	const canvas = await html2canvas(element as HTMLElement, { scale: 1 })
	const a = document.createElement('a')
	a.href = canvas.toDataURL('image/jpeg')
	a.download = 'note.png'
	a.click()
}
</script>

<template>
	<div class="w-full h-screen">
		<div class="flex items-center gap-3">
			<input v-model="angle" type="number" name="" id="">
			<button @click="downloadImage">Download</button>
			<button @click="preview = !preview">Preview</button>
		</div>
		<div class="w-full grid grid-cols-1 sm:grid-cols-2 gap-3 py-3">
			<textarea v-model="text" name="" id="" cols="30" rows="30"></textarea>
			<div v-show="preview" class="page p-8 w-595px h-842px relative ">
				<div class="absolute top-0 left-0 w-full h-full z-10"
					:style="background"></div>
				<template v-for="word, index in words"
					:key="`word-${word}-${index}`">
					<template v-if="word == '\\n'">
						<p class="block my-1"></p>
					</template>
					<template v-else-if="word == '\\t'">
						<span class="inline-block w-20px"></span>
					</template>
					<template v-else>
						<p class="mx-1 my-0 inline-block">
							<span v-for="letter, i in word"
								:key="`letter-${word}-${i}`"
								:style="letterStyle(letter)">
								{{ letter }}
							</span>
						</p>
					</template>
				</template>
			</div>
		</div>
	</div>
</template>

<style scoped>
.page {
	font-family: 'Cedarville Cursive', cursive;
	aspect-ratio: 1/1.4142;
	background: linear-gradient(to right, #eee, #ddd);
}
</style>