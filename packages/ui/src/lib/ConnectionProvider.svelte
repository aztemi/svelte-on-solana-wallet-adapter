<script lang="ts">
	import { Connection } from '@solana/web3.js';
	import type { Commitment, ConnectionConfig } from '@solana/web3.js';
	import { workSpace } from './workSpace';

	export let endpoint: string,
		config: Commitment | ConnectionConfig | undefined = 'confirmed';

	const connection = new Connection(endpoint, config);

	workSpace.set({ connection });
</script>
