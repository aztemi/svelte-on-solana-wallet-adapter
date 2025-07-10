<script lang="ts">
	import { Connection } from '@solana/web3.js';
	import type { Commitment, ConnectionConfig } from '@solana/web3.js';
	import { workSpace } from './workSpace';
	import { web3, Program, AnchorProvider } from '@coral-xyz/anchor';
	import { walletStore } from '@aztemi/svelte-on-solana-wallet-adapter-core';

	export let idl,
		endpoint: string,
		config: Commitment | ConnectionConfig | undefined = 'confirmed';

	const { PublicKey } = web3;
	const programID = new PublicKey(idl.metadata.address);
	const baseAccount = web3.Keypair.generate();
	const systemProgram = web3.SystemProgram;
	const connection = new Connection(endpoint, config);

	function defineProgramAndProvider(walletStore) {
		let { sendTransaction, signTransaction, signAllTransactions, signMessage, publicKey } =
			walletStore;
		const providerWallet = {
			sendTransaction,
			signTransaction,
			signAllTransactions,
			signMessage,
			publicKey
		};
		const provider = new AnchorProvider(connection, providerWallet, {
			preflightCommitment: 'processed'
		});
		const program = new Program(idl, programID, provider);
		workSpace.set({
			baseAccount,
			connection,
			provider,
			program,
			systemProgram,
			endpoint
		});
		return {
			baseAccount,
			connection,
			provider,
			program,
			systemProgram,
			endpoint
		};
	}

	$: $walletStore && $walletStore.publicKey && defineProgramAndProvider($walletStore);
</script>

<slot />
