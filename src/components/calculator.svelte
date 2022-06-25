<script>
  // @ts-nocheck
  import Switch from "@smui/switch";
  import { overlayVisible } from "../store";
  const currency = "$";
  let clicked = false;
  let ctragreed = false;
  let upsellagreed = false;
  let traffic,
    cpc,
    ctr,
    price,
    cost,
    cr,
    uprice,
    ucost,
    ucr,
    cconf,
    cordconfcost,
    shippingdeliv,
    shippingcost;

  const trafficCost = (traffic, cpc) => {
    return traffic * cpc;
  };
  const productSpending = (cost, traffic, cr, cconf) => {
    return cost * (traffic * (cr / 100) * (cconf / 100));
  };
  const salePageConversion = (traffic, cr) => {
    return Math.round(traffic * (cr / 100));
  };
  const callCenterFees = (traffic, cr, cordconfcost) => {
    return Math.round(traffic * (cr / 100) * cordconfcost);
  };
  const totalOrderConfirmed = (traffic, cr, cconf) => {
    return Math.round(salePageConversion(traffic, cr) * (cconf / 100));
  };
  const totalShippingFees = (traffic, cr, cconf, shippingcost) => {
    return Math.round(totalOrderConfirmed(traffic, cr, cconf) * shippingcost);
  };
  const totalOrderDelivered = (traffic, cr, cconf, shippingdeliv) => {
    return Math.round(
      totalOrderConfirmed(traffic, cr, cconf) * (shippingdeliv / 100)
    );
  };
  const totalProductsDelivered = (traffic, cr, cconf, shippingdeliv, cost) => {
    return totalOrderDelivered(traffic, cr, cconf, shippingdeliv) * cost;
  };
  const costProductReturned = (traffic, cr, cconf, shippingdeliv, cost) => {
    return (
      (totalOrderConfirmed(traffic, cr, cconf) -
        totalOrderDelivered(traffic, cr, cconf, shippingdeliv)) *
      cost
    );
  };
  const totalRevenue = (traffic, cr, cconf, shippingdeliv, price) => {
    return totalOrderDelivered(traffic, cr, cconf, shippingdeliv) * price;
  };
  const totalNetProfit = (
    traffic,
    cr,
    cpc,
    cconf,
    shippingdeliv,
    shippingcost,
    price,
    cost,
    cordconfcost
  ) => {
    return (
      totalRevenue(traffic, cr, cconf, shippingdeliv, price) -
      totalProductsDelivered(traffic, cr, cconf, shippingdeliv, cost) -
      totalShippingFees(traffic, cr, cconf, shippingcost) -
      callCenterFees(traffic, cr, cordconfcost) -
      trafficCost(traffic, cpc)
    );
  };
  function calculate() {
    clicked = true;
    $overlayVisible = true;
  }
</script>

<div class="cols">
  <ul>
    <li class="traffic-cpc">
      <div>
        <p>traffic</p>
        <input
          type="text"
          placeholder="What's your traffic"
          bind:value={traffic}
          required
        />
      </div>
      <div>
        <p>CPC</p>
        <input
          type="text"
          placeholder="What's your CPC"
          bind:value={cpc}
          required
        />
      </div>
    </li>

    <li class="ctr" class:disabled={ctragreed === false}>
      <div class="switch">
        <Switch bind:checked={ctragreed} />
      </div>
      <h3>Landing Page</h3>
      <p>CTR %</p>
      <input
        type="text"
        placeholder="What's your CTR?"
        bind:value={ctr}
        disabled={!ctragreed}
      />
    </li>

    <li class="sale">
      <h3>Sale Page</h3>
      <p>Product price</p>
      <input
        type="text"
        placeholder="What's your Product Price?"
        bind:value={price}
        required
      />
      <p>Product cost</p>
      <input
        type="text"
        placeholder="What's your Product Cost?"
        bind:value={cost}
        required
      />
      <p>Conversion rate</p>
      <input
        type="text"
        placeholder="What's your Conversion Rate?"
        bind:value={cr}
        required
      />
    </li>
    <li class="upsell" class:disabled={upsellagreed === false}>
      <div>
        <div class="switch">
          <Switch bind:checked={upsellagreed} />
        </div>
        <h3>Upsell</h3>
      </div>
      <div>
        <p>Product price</p>
        <input
          type="text"
          placeholder="What's your CTR?"
          bind:value={uprice}
          disabled={!upsellagreed}
        />
        <p>Product cost</p>
        <input
          type="text"
          placeholder="What's your CTR?"
          bind:value={ucost}
          disabled={!upsellagreed}
        />
        <p>Conversion rate</p>
        <input
          type="text"
          placeholder="What's your CTR?"
          bind:value={ucr}
          disabled={!upsellagreed}
        />
      </div>
    </li>
    <li class="thank-you">
      <h3>Thank you Page</h3>
      <button class="buttonClass" on:click={calculate}>Go</button>
    </li>
  </ul>

  <div class="call-center">
    <div>
      <p>Call Center Confirmation Rate %</p>
      <input
        type="text"
        placeholder="Call center confirmation Rate %"
        bind:value={cconf}
        required
      />
    </div>
    <div>
      <p>Shipping deliverability %</p>
      <input
        type="text"
        placeholder="Shipping deliverability %"
        bind:value={shippingdeliv}
        required
      />
    </div>
    <div>
      <p>Call center order confirmation cost</p>
      <input
        type="text"
        placeholder="Call center order confirmation cost"
        bind:value={cordconfcost}
        required
      />
    </div>
    <div>
      <p>Shipping cost</p>
      <input
        type="text"
        placeholder="Shipping cost"
        bind:value={shippingcost}
        required
      />
    </div>
  </div>
</div>
{#if clicked}
  <div class="results">
    <h1>Results</h1>
    <div class="answers">
      <div class="answer">
        <span>Total Traffic Spending</span>
        <h5>{trafficCost(traffic, cpc)},00{currency}</h5>
      </div>
      <div class="answer">
        <span>Estimated Total Product Spending</span>
        <h5>{productSpending(cost, traffic, cr, cconf)},00{currency}</h5>
      </div>
      <div class="answer">
        <span>Sales Page Conversion</span>
        <h5>{salePageConversion(traffic, cr)}</h5>
      </div>
      <div class="answer">
        <span>Total Call Center Fees</span>
        <h5>{callCenterFees(traffic, cr, cordconfcost)},00{currency}</h5>
      </div>
      <div class="answer">
        <span>Total Shipping Fees</span>
        <h5>
          {totalShippingFees(traffic, cr, cconf, shippingcost)},00{currency}
        </h5>
      </div>
      <div class="answer">
        <span>Total Order got Confirmed / Shipped</span>
        <h5>{totalOrderConfirmed(traffic, cr, cconf)}</h5>
      </div>
      <div class="answer">
        <span>Total Order Delivered</span>
        <h5>
          {totalOrderDelivered(traffic, cr, cconf, shippingdeliv)}
        </h5>
      </div>
      <div class="answer">
        <span>Total Cost of Delivered Products</span>
        <h5>
          {totalProductsDelivered(
            traffic,
            cr,
            cconf,
            shippingdeliv,
            cost
          )},00{currency}
        </h5>
      </div>
      <div class="answer">
        <span>Total Cost of Returned Products</span>
        <h5>
          {costProductReturned(
            traffic,
            cr,
            cconf,
            shippingdeliv,
            cost
          )},00{currency}
        </h5>
      </div>
    </div>

    <div class="revs">
      <div class="rev blue">
        <span>Total Revenue</span>
        <h5>
          {totalRevenue(traffic, cr, cconf, shippingdeliv, price)},00{currency}
        </h5>
      </div>
      <div
        class="rev"
        class:red={totalNetProfit(
          traffic,
          cr,
          cpc,
          cconf,
          shippingdeliv,
          shippingcost,
          price,
          cost,
          cordconfcost
        ) < 0}
        class:green={totalNetProfit(
          traffic,
          cr,
          cpc,
          cconf,
          shippingdeliv,
          shippingcost,
          price,
          cost,
          cordconfcost
        ) > 0}
      >
        <span>Total Net Profit</span>
        <h5>
          {totalNetProfit(
            traffic,
            cr,
            cpc,
            cconf,
            shippingdeliv,
            shippingcost,
            price,
            cost,
            cordconfcost
          )},00{currency}
        </h5>
      </div>
    </div>
  </div>
{/if}

<style>
  * {
    box-sizing: border-box;
  }
  .cols {
    background-color: #eaf8fe;
  }
  ul {
    padding: 25px;
    margin: auto;
    list-style-type: none;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-gap: 0;
  }
  li {
    flex: 1 auto;
    background-color: #fff;
    border: 1px solid #fff;
    padding: 30px 15px;
    border-right: 1px solid #ddd;
    -webkit-box-shadow: 21px 21px 41px -3px rgba(0, 0, 0, 0.43);
    -moz-box-shadow: 21px 21px 41px -3px rgba(0, 0, 0, 0.43);
    box-shadow: 21px 21px 41px -3px rgba(0, 0, 0, 0.43);
  }
  li {
    padding-top: 70px;
  }
  li h3 {
    text-align: center;
    color: #006fba;
  }
  input {
    color: #006fbab3;
    height: 50px;
    font-size: 13px;
    outline: none;
    padding: 0 20px;
    border: 1px solid #006fbab3;
    width: 100%;
    border-radius: 0;
    box-shadow: none;
    overflow: visible;
  }
  ::placeholder {
    color: #006fba;
  }
  .switch {
    text-align: center;
  }
  .thank-you {
    text-align: center;
  }
  .buttonClass {
    width: 100%;
    height: 50px;
    color: #fff;
    background-color: #006fba;
    border: 1px #006fba solid;
    text-shadow: 1px 1px 0px #2f6627;
  }

  .buttonClass:hover {
    opacity: 75%;
    cursor: pointer;
  }
  .disabled {
    background-color: #f3f3f3 !important;
  }
  .call-center {
    max-width: 775px;
    margin: auto;
    padding: 30px;
    grid-gap: 15px;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
  .results {
    text-align: center;
    background-color: #f7fdff;
    padding: 50px 0;
  }
  .answers {
    display: grid;
    grid-gap: 15px;
    grid-template-columns: repeat(2, 1fr);
    width: 650px;
    margin: auto auto 20px;
  }
  .answer {
    text-align: center;
    background-color: #fff;
    box-shadow: 10px 12px 40px 0 rgb(6 50 80 / 7%);
    padding: 25px 35px;
  }
  .answer span {
    color: #07283e;
    font-size: 14px;
  }
  .answer h5 {
    line-height: 1.2;
    font-size: 1.125rem;
    color: #07283e;
    font-weight: 600;
    margin-bottom: 0;
    margin-top: 5px;
  }
  .revs {
    display: grid;
    grid-gap: 15px;
    width: 560px;
    margin: auto auto 20px;
    margin-bottom: 0;
  }
  .rev {
    text-align: center;
    box-shadow: 10px 12px 40px 0 rgb(6 50 80 / 7%);
    padding: 22px 35px;
  }
  .blue {
    background-color: #006fba;
  }
  .rev span {
    font-size: 14px;
    color: #fff;
  }
  .rev h5 {
    line-height: 1.2;
    font-size: 25px;
    color: #fff;
    font-weight: 600;
    margin-bottom: 0;
    margin-top: 5px;
  }
  .red {
    background-color: #d64545;
  }
  .green {
    background-color: #13ba00;
  }
  @media (max-width: 1024px) {
    ul {
      grid-template-columns: repeat(3, 1fr);
    }
  }
  @media (max-width: 700px) {
    ul {
      grid-template-columns: repeat(2, 1fr);
    }
  }
  @media (max-width: 500px) {
    ul {
      grid-template-columns: repeat(1, 1fr);
    }
    .call-center {
      max-width: 100%;
      grid-template-columns: repeat(1, 1fr);
    }
  }
</style>
