<script lang="ts">
	import type { Stocks } from './types';

	const fetchStocks: Promise<Stocks> = (async () => {
		const resp = await fetch(
			'https://brapi.dev/api/quote/list?sortBy=close&sortOrder=desc&limit=10'
		);
		return await resp.json();
	})();
</script>

{#await fetchStocks}
	<p>...waiting</p>
{:then data}
	<table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr>
                <th class="py-3 px-6">Ação</th>
                <th colspan="2" class="py-3 px-6">Empresa</th>
                <th class="py-3 px-6">Preço</th>
                <th class="py-3 px-6">Ações</th>
            </tr>
        </thead>
		<tbody>
			{#each data.stocks as stock}
				<tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
					<td class="py-2 px-6">{stock.stock}</td>
					<td class="py-2 px-6"><img src={stock.logo} class="w-12 h-12" alt={stock.stock} /></td>
					<td class="py-2 px-6">{stock.name}</td>
					<td class="py-2 px-6">
						{new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(
							stock.close
						)}
					</td>
                    <td class="py-2 px-6">
                        <button type="button" class="bg-blue-800 py-2 px-4 text-white">Comprar</button>
                    </td>
				</tr>
			{/each}
		</tbody>
	</table>
{:catch error}
	<p>An error occurred!</p>
{/await}
