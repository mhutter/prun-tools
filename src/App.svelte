<script>
  let input;

  $: rawData = JSON.parse(atob(input || "e30="));
  $: data = (rawData.materialPrices || [])
    .filter((i) => i.NC1)
    .map((i) => {
      const { ticker } = i;
      const ask = (i.NC1.ask || { price: 9999 }).price;
      const bid = (i.NC1.bid || { price: 0 }).price;
      return { ticker, ask, bid };
    })
    .sort((a, b) => a.ticker.localeCompare(b.ticker));
  $: console.log(data);
</script>

<h1>Prices</h1>
<p>
  Eval see <a
    href="https://raw.githubusercontent.com/rain9441/prun-data-extraction/master/extraction-evals/prices.eval"
    >https://raw.githubusercontent.com/rain9441/prun-data-extraction/master/extraction-evals/prices.eval</a
  >
</p>
<div>
  <p>Paste data here:</p>
  <input type="text" bind:value={input} />
</div>
<div>
  <p>=====</p>
  <table>
    <tr>
      <th>Ticker</th>
      <th>Ask</th>
      <th>Bid</th>
    </tr>
    {#each data as { ticker, ask, bid }}
      <tr>
        <td>{ticker}</td>
        <td>{ask}</td>
        <td>{bid}</td>
      </tr>
    {/each}
  </table>
  <p>=====</p>
</div>
