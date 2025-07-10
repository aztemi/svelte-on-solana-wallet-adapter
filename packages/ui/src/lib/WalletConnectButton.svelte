<script lang="ts">
	import { walletStore } from '@aztemi/svelte-on-solana-wallet-adapter-core';
	import WalletButton from './WalletButton.svelte';

	export let disabled: boolean = false;

	let content: string;

	$: ({ wallet, connect, connecting, connected } = $walletStore);

	$: {
		content = 'Connect Wallet';
		if (wallet) content = 'Connect';
		if (connecting) content = 'Connecting ...';
		if (connected) content = 'Connected';
	}

	function handleClick(e: MouseEvent) {
		connect().catch(() => {});
	}
</script>

<WalletButton
	on:click={handleClick}
	disabled={disabled || !wallet || connecting || connected}
	class="wallet-adapter-button-trigger"
>
	<svelte:fragment slot="start-icon">
		{#if wallet}
			<img src={wallet.adapter.icon} alt={`${wallet.adapter.name} icon`} />
		{/if}
	</svelte:fragment>
	{content}
</WalletButton>
