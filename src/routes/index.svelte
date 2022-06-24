<script>
  import Button, { Label } from "@smui/button";
</script>

<ul>
  <li><a href="/"><img src="ecomSimulatorLogo.svg" alt="" /></a></li>
  <li>
    <h1>
      The best E-commerce <br />
      business <span>simulator</span>
    </h1>
  </li>
  <li>
    <p>
      Try it now and get the results you need to <br />
      start your own e-commerce business
    </p>
  </li>
  <li>
    <a href="/simulator" id="btn">Start Now</a>
  </li>
</ul>

<style>
  :global(html, body) {
    background: #c0e8f9;
  }
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Open Sans", sans-serif;
  }
  ul {
    padding-top: 250px;
    display: flex;
    list-style-type: none;
    flex-direction: column;
  }
  li {
    padding-bottom: 20px;
    flex: 1;
    text-align: center;
  }
  li img {
    margin-bottom: 20px;
  }
  li h1 {
    font-size: 50px;
    margin-bottom: 20px;
  }
  li h1 span {
    color: #006fba;
  }
  li p {
    font-size: 18px;
    color: rgba(7, 40, 62, 0.6);
    margin-bottom: 20px;
  }
  #btn {
    color: white;
    font-size: 14px;
    background: #006fba;
    padding: 15px 65px;
    text-decoration: none;
  }
  @media only screen and (max-width: 600px) {
    li h1 {
      font-size: 35px;
    }
  }
</style>
