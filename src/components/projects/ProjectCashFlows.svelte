<script>
  export let cashflows = [];

  let maxValue = 0;
  let scaleFactor = 1;
  let containerHeight = 300;

  $: data = cashflows.map((cashflow) => {
    const netCashflow = cashflow.positiveCF - cashflow.negativeCF;
    maxValue =
      Math.abs(netCashflow) > maxValue ? Math.abs(netCashflow) : maxValue;
    scaleFactor = (containerHeight * 0.5) / maxValue;
    return {
      ...cashflow,
      netCashflow,
    };
  });

  function handleCashflowChange(cashflowType, value, index) {
    const cashflowToUpdate = cashflows[index];
    cashflowToUpdate[cashflowType] = value;
    cashflows = [
      ...cashflows.slice(0, index),
      cashflowToUpdate,
      ...cashflows.slice(index + 1),
    ];
  }
</script>

<style>
  .cashflows-container {
    position: relative;
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: space-between;
  }

  .cashflows-timeline {
    width: 95%;
    height: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }

  .cashflow {
    display: flex;
    align-items: center;
    position: relative;
    max-width: 80px;
  }

  .net-cashflow {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
  }

  .net-cashflow.positive {
    background-color: #48c774;
    bottom: 50%;
  }

  .net-cashflow.negative {
    background-color: tomato;
    top: 50%;
  }
</style>

<div class="cashflows-container" style={`height: ${containerHeight}px`}>
  <div class="cashflows-timeline has-background-dark" />
  {#each data as cashflow, index}
    <div class="cashflow">
      <div
        class="net-cashflow {cashflow.netCashflow < 0 ? 'negative' : 'positive'}"
        style={`width: 20px; height: ${Math.abs(cashflow.netCashflow) * scaleFactor}px`} />
      <div class="cashflow-fields">
        <input
          class="input is-small"
          value={cashflow.positiveCF}
          on:input={(event) => handleCashflowChange('positiveCF', +event.target.value, index)}
          type="number"
          placeholder="+CF" />
        <input
          class="input is-small"
          value={cashflow.negativeCF}
          on:input={(event) => handleCashflowChange('negativeCF', +event.target.value, index)}
          type="number"
          placeholder="-CF" />
      </div>
    </div>
  {/each}
</div>
