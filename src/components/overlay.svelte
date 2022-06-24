<script>
  import { visibility } from "../store";
  var progress = 0;

  /**
   * @type {string | number | NodeJS.Timeout | undefined}
   */
  let prog; //for setInterval
  const addColor = () => (progress += 1);
  const randomTime = () => Math.floor(Math.random() * 50);
  $: if (progress === 100) {
    stopProgress();
    setTimeout(resetBar, 2500);
    $visibility = "hidden";
  }
  const stopProgress = () => {
    clearInterval(prog);
  };
  const resetBar = () => {
    stopProgress();
    progress = 0;
  };
  const makeProgress = () => {
    prog = setInterval(addColor, randomTime());
  };
  makeProgress();
</script>

<div class="overlay" style="visibility: {$visibility};">
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
  * {
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
