<script lang="ts">
	import BackgroundVideo from "$lib/components/BackgroundVideo.svelte";
	import discordIcon from "$lib/images/discord-mark-white.svg";
	import xIcon from "$lib/images/x-logo.svg";
	import ghIcon from "$lib/images/github-icon.svg";
	import patreonIcon from "$lib/images/patreon-icon.svg";
	import beatRunning from "$lib/images/beat_running.gif"; 

	let iconElement: HTMLImageElement;
	let iconContainer: HTMLDivElement;

	const magnetEffect = (e: MouseEvent) => {
		e.stopPropagation();

		const { offsetX: x, offsetY: y } = e;
		const { offsetWidth: width, offsetHeight: height } = iconContainer;

		const move = 20;
		const xMove = (x / width) * (move * 2) - move;
		const yMove = (y / height) * (move * 2) - move;

		iconElement.style.transform = `translate(${xMove}px, ${yMove}px)`;

		if (e.type === "mouseleave"){ iconElement.style.transform = ""; }
	};

	const getLatestInstallerDownloadUrl = async (): Promise<string> => {
		const res = await fetch("https://api.github.com/repos/Zagrios/bs-manager/releases/latest", { signal: AbortSignal.timeout(5000) });
		const data = await res.json();
		return data.assets.find((asset: {name: string, browser_download_url: string}) => /BSManager-Setup-(\d+\.\d+\.\d+)\.exe/.test(asset.name))?.browser_download_url;
	}

	const openDownloadPage = () => {
		const downloadUrlFallback = "https://github.com/Zagrios/bs-manager/releases/latest";
		getLatestInstallerDownloadUrl().then(url => {
			if (url) { return window.location.href = url; }
			throw new Error("No download url found for the latest installer");
		}).catch(() => {
			window.open(downloadUrlFallback, "_blank");
		});
	}

</script>

<svelte:head>
	<title>BSManager</title>
	<meta name="description" content="BSManager website" />
</svelte:head>

<section class="w-dvw h-dvh overflow-hidden">

	<BackgroundVideo class="size-full absolute top-0 left-0 -z-10"/>
	<div class="size-full flex justify-center items-center flex-col z-30 gap-5 px-3 [&>*]:max-w-full">
		<div bind:this={iconContainer} on:mousemove={magnetEffect} on:mouseleave={magnetEffect} class="size-48 sm:size-96 max-h-full max-w-full" role="form">
			<img bind:this={iconElement} id="bsm-icon" class="size-full shadow-black drop-shadow-md ease-in-out duration-300" src="favicon.svg" alt="BSManager icon"/>
		</div>
		<button on:click|preventDefault="{openDownloadPage}" class="bg-[#3b82ff] bg-opacity-30 cursor-pointer backdrop-blur-xl h-20 w-[30rem] rounded-2xl text-white shadow-black shadow-md font-bold text-2xl sm:text-3xl  flex flex-row justify-evenly items-center mt-5 hover:w-[35rem] transition-all duration-100 ease-in-out">
			<img src="{beatRunning}" class="size-11 mt-0.5" alt="gif"/>
			<span class="text-center">Download BSManager</span>
			<img src="{beatRunning}" class="size-11 mt-0.5" alt="gif"/>
		</button>
		<h2 class="text-white font-bold text-3xl">Follow us !</h2>
		<section class="flex flex-row flex-wrap justify-center items-center gap-7 w-[35rem] [&>a]:shadow-black [&>a]:shadow-md [&>a]:shrink-0 [&>a]:transition-transform">
			<a href="https://discord.gg/bsmanager-1049624409276694588" target="_blank" class="size-16 p-2.5 bg-[#5865F2] rounded-2xl cursor-pointer z-10 hover:-translate-y-1" title="Discord">
				<img class="size-full" src="{discordIcon}" alt="discord icon"/>
			</a>
			<a href="https://twitter.com/BSManager_" target="_blank" class="size-16 p-3.5 bg-neutral-900 rounded-2xl cursor-pointer z-10 hover:-translate-y-1" title="Twitter">
				<img class="size-full" src="{xIcon}" alt="twitter icon"/>
			</a>
			<a href="https://github.com/Zagrios/bs-manager" target="_blank" class="size-16 p-2.5 bg-neutral-900 rounded-2xl cursor-pointer z-10 hover:-translate-y-1" title="GitHub">
				<img class="size-full" src="{ghIcon}" alt="github icon"/>
			</a>
			<a href="https://www.patreon.com/bsmanager" target="_blank" class="size-16 p-2.5 bg-white rounded-2xl cursor-pointer z-10 hover:-translate-y-1" title="Patreon">
				<img class="size-full" src="{patreonIcon}" alt="patreon icon"/>
			</a>
		</section>
		<p class="text-gray-400 whitespace-break-spaces text-center mt-3 sm:mt-10">Add this site to your favorites and stay tuned! This is work in progress, and for now, it's used only as temporary content.</p>
	</div>

	<div class="absolute bottom-4 right-4 text-neutral-200 flex justify-center items-center gap-3 *:underline *:underline-offset-2">
		<a href="https://beatsaver.com/maps/16992" target="_blank">See map</a>
		<a href="beatsaver://16992">Install map</a>
	</div>

</section>

<style>

	#bsm-icon {
		transition: transform 300ms cubic-bezier(0.3, 1, 0.7, 1);
	}

</style>
