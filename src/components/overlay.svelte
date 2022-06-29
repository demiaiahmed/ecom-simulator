<script>
  import { overlayVisible } from "../store";
  var progress = 0;
  /**
   * @type {any}
   */
  let vis;

  /**
   * @type {string | number | NodeJS.Timeout | undefined}
   */
  let prog; //for setInterval
  const addColor = () => (progress += 1);
  const randomTime = () => Math.floor(Math.random() * 60);

  const stopProgress = () => {
    clearInterval(prog);
  };
  const resetBar = () => {
    stopProgress();
    progress = 0;
  };
  const makeProgress = () => {
    setInterval(addColor, 70);
  };
  makeProgress();
  $: if ($overlayVisible) {
    vis = "visible";
  } else {
    vis = "hidden";
  }

  $: if (progress === 100) {
    stopProgress();
    setTimeout(resetBar, 500);
    $overlayVisible = false;
  }
</script>

<div class="overlay" style="--vis:{vis};">
  <div class="cont">
    <div class="img">
      <img src="calculator-shadow.png" alt="" />
    </div>
    <h1>Thank you for Waiting</h1>
    <p>We are calculating your estimations</p>
    <div class="outer">
      <div class="inner" style="width:{progress}%" />
    </div>
  </div>
  <span>{progress}%</span>
</div>

<style>
  :global(html, body) {
    background: #fff;
  }
  .overlay {
    height: 100%;
    width: 100%;
    background-color: #fff;
    background-image: url(bg.svg);
    background-position: -10px -40px;
    background-size: cover;
    background-repeat: no-repeat;
    overflow: hidden;
    position: absolute;
    top: 0;
    left: 0;
    align-items: center;
    justify-content: center;
    z-index: 1;
    text-align: center !important;
    visibility: var(--vis);
  }
  .cont {
    padding-top: 350px;
  }
  .img {
    width: 195px;
    margin: auto auto 62px;
  }
  .img img {
    border-style: none;
    vertical-align: middle;
    width: 100%;
  }
  h1 {
    font-size: 46px;
    margin-bottom: 18px;
    font-weight: 500;
    color: #07283e;
  }
  p {
    color: rgba(7, 40, 62, 0.6);
    font-size: 18px;
    font-weight: 400;
    margin-bottom: 44px;
  }
  .outer,
  .inner {
    height: 10px;
    border-radius: 10px;
  }
  .outer {
    width: 250px;
    margin: 30px auto;
    background-color: #c3c3c3;
  }
  .inner {
    background: #77b2d7;
  }
  span {
    color: #77b2d7;
  }
</style>
