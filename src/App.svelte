<script>
  import Button from './lib/Button.svelte';
  import Dropdown from './lib/Dropdown.svelte';
  import Preview from './lib/Preview.svelte';
  import Result from './lib/Result.svelte';

  // default settings
  let bandCount = 3;
  let digits = ['1', '0'];
  let multiplier = 1;
  let tolerance = 1;
  let tempcoeff = 100;
  let result = '';
  let resetDropdowns = false;

  function handleChange(menu) {
    console.log(menu);
    switch (menu[0]) {
      case '你要幾色環':
        // update to defaults if band count is changed
        if (bandCount > 3 && menu[1] == 3) {
          tolerance = 1;
        }
        if (bandCount == 6 && menu[1] < 6) {
          tempcoeff = 100;
        }
        if (bandCount < 5 && menu[1] >= 5) {
          digits.push('0');
        } else if (bandCount >= 5 && menu[1] < 5) {
          digits.pop();
        }

        bandCount = menu[1];
        break;
        case '第一色環':
        digits[0] = menu[1].toString();
        break;
      case '第二色環':
        digits[1] = menu[1].toString();
        break;
      case '第三色環':
        digits[2] = menu[1].toString();
        break;
      case '相乘倍數':
        multiplier = menu[1];
        break;
      case '誤差值':
        tolerance = menu[1];
        break;
      case '溫度係數':
        tempcoeff = menu[1];
        break;
    }
  }

  function updateResult(text) {
    result = text;
  }

  async function copyText() {
    await navigator.clipboard.writeText(result);
    alert('複製電阻資訊成功!');
  }
</script>

<main>
  {#key [bandCount, digits, multiplier, tolerance, tempcoeff]}
    <Preview {bandCount} {digits} {multiplier} {tolerance} {tempcoeff} />
    <Result {bandCount} {digits} {multiplier} {tolerance} {tempcoeff} on:resultUpdate={(e) => updateResult(e.detail)} />
  {/key}
  <div class="buttons">
    <Button text="<i class='fa-solid fa-copy'></i>" on:click={copyText} />
  </div>
  {#key resetDropdowns}
    <div class="dropdowns">
      <Dropdown on:selectChange={(e) => handleChange(e.detail)} data={[
        [3, '三色環'],
        [4, '四色環'],
        [5, '五色環'],
        [6, '六色環'],
      ]} name="你要幾色環" />

      <Dropdown on:selectChange={(e) => handleChange(e.detail)} data={[
        [1, '棕色', '#8C4D19'],
        [2, '紅色', '#DE4439'],
        [3, '橙色', '#EF8228'],
        [4, '黃色', '#FCD500'],
        [5, '綠色', '#53C638'],
        [6, '藍色', '#1A94D0'],
        [7, '紫色', '#7E43B1'],
        [8, '灰色', '#999999'],
        [9, '白色', '#FFFFFF'],
      ]} name="第一色環" />
      
      <Dropdown on:selectChange={(e) => handleChange(e.detail)} data={[
        [0, '黑色', '#000000'],
        [1, '棕色', '#8C4D19'],
        [2, '紅色', '#DE4439'],
        [3, '橘色', '#EF8228'],
        [4, '黃色', '#FCD500'],
        [5, '綠色', '#53C638'],
        [6, '藍色', '#1A94D0'],
        [7, '紫色', '#7E43B1'],
        [8, '灰色', '#999999'],
        [9, '白色', '#FFFFFF'],
      ]} name="第二色環" />

      {#if bandCount >= 5}
        <Dropdown on:selectChange={(e) => handleChange(e.detail)} data={[
          [0, '黑色', '#000000'],
          [1, '棕色', '#8C4D19'],
          [2, '紅色', '#DE4439'],
          [3, '橙色', '#EF8228'],
          [4, '黃色', '#FCD500'],
          [5, '綠色', '#53C638'],
          [6, '藍色', '#1A94D0'],
          [7, '紫色', '#7E43B1'],
          [8, '灰色', '#999999'],
          [9, '白色', '#FFFFFF'],
        ]} name="第三色環" />
      {/if}
      
      <Dropdown on:selectChange={(e) => handleChange(e.detail)} data={[
        [1, '黑色', '#000000'],
        [10, '棕色', '#8C4D19'],
        [100, '紅色', '#DE4439'],
        [1000, '橙色', '#EF8228'],
        [10000, '黃色', '#FCD500'],
        [100000, '綠色', '#53C638'],
        [1000000, '藍色', '#1A94D0'],
        [10000000, '紫色', '#7E43B1'],
        [100000000, '灰色', '#999999'],
        [1000000000, '白色', '#FFFFFF'],
        [0.1, '金色', '#FFB800'],
        [0.01, '銀色', '#DCDCDC']
      ]} name="相乘倍數" />

      {#if bandCount >= 4}
        <Dropdown on:selectChange={(e) => handleChange(e.detail)} data={[
          [1, '棕色', '#8C4D19'],
          [2, '紅色', '#DE4439'],
          [0.5, '綠色', '#53C638'],
          [0.25, '藍色', '#1A94D0'],
          [0.10, '紫色', '#7E43B1'],
          [0.05, '灰色', '#999999'],
          [5, '金色', '#FFB800'],
          [10, '銀色', '#DCDCDC']
        ]} name="誤差值" />
      {/if}

      {#if bandCount == 6}
        <Dropdown on:selectChange={(e) => handleChange(e.detail)} data={[
          [100, '棕色', '#8C4D19'],
          [50, '紅色', '#DE4439'],
          [15, '橙色', '#EF8228'],
          [25, '黃色', '#FCD500'],
          [10, '藍色', '#1A94D0'],
          [5, '紫色', '#7E43B1'],
        ]} name="溫度係數" />
      {/if}
    </div>
  {/key}
</main>

<style>

  .dropdowns {
    display: flex;
    flex-direction: row;
    justify-content: center;
  }

  @media screen and (max-width: 1000px) {
    .dropdowns {
      flex-direction: column;
      justify-content: center;
    }
  }

  .buttons {
    max-width: 200px;
    margin: auto auto 15px auto;
  }
</style>