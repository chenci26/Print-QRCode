<template>
  <div class="home">
    <div class="title no-print">
      請輸入內容
    </div>
    <textarea
      autofocus
      class="input no-print"
      v-model="content"
    />
    <div class="size no-print">
      <div class="size-title">
        QRCODE寬(預設100%)
      </div>
      <input
        class="size-input no-print"
        v-model="qrWidth"
        type="text"
      >
      <div class="size-title">
        QRCODE高(預設100%)
      </div>
      <input
        class="size-input no-print"
        v-model="qrHeight"
        type="text"
      >
      <div class="size-title">
        列印紙張大小(預設A4)
      </div>
      <input
        class="size-input no-print"
        v-model="printSize"
        type="text"
      >
      <div class="size-title">
        列印文字內容大小(預設16px)
      </div>
      <input
        class="size-input no-print"
        v-model="contentSize"
        type="text"
      >
      <div class="size-title">
        <input
          v-model="showContent"
          type="checkbox"
          checked
        >
        列印是否顯示內容
      </div>
    </div>
    <div
      v-if="!qrImgSrc"
      class="qrcode no-print"
    >
      請輸入內容轉換成QRCODE
    </div>
    <img
      v-else
      class="qrcode"
      :src="qrImgSrc"
      alt=""
    >
    <div
      class="hide-content"
      :class="{'show-content' : showContent}"
    >
      {{ content }}
    </div>
    <div class="btn-wrapper no-print">
      <div
        class="btn"
        @click="create"
      >
        產生QRCode
      </div>
      <div
        class="btn"
        @click="print"
      >
        列印
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import QRCode from 'qrcode';

export default {
  setup() {
    const content = ref('');
    const showContent = ref(true);
    const contentSize = ref('16px');
    const qrImgSrc = ref('');
    const qrWidth = ref('100%');
    const qrHeight = ref('100%');
    const printSize = ref('A4');

    const handleCssVars = () => {
      const rootStyle = document.documentElement.style;
      if (!contentSize.value) contentSize.value = '16px';
      if (!qrWidth.value) qrWidth.value = '100%';
      if (!qrHeight.value) qrHeight.value = '100%';
      if (!printSize.value) printSize.value = 'A4';
      rootStyle.setProperty('--content-size', contentSize.value);
      rootStyle.setProperty('--qrcode-width', qrWidth.value);
      rootStyle.setProperty('--qrcode-height', qrHeight.value);
      rootStyle.setProperty('--print-size', printSize.value);
    };

    const create = async () => {
      if (!content.value) {
        alert('輸入內容啦幹!!!!!!!!!!!!!');
        return;
      }
      await handleCssVars();
      const QROptions = {
        margin: 1,
        errorCorrectionLevel: 'L',
      };
      qrImgSrc.value = await QRCode.toDataURL(content.value, QROptions);
    };

    const print = async () => {
      window.print();
    };

    return {
      content,
      showContent,
      contentSize,
      qrWidth,
      qrHeight,
      qrImgSrc,
      printSize,
      create,
      print,
    };
  },
};
</script>

<style lang="scss" scoped>
@media print {
  * {
    -webkit-print-color-adjust: exact !important;
  }

  .home {
    padding: 0 !important;

    .no-print {
      display: none !important;
    }

    .qrcode {
      width: var(--qrcode-width) !important;
      height: var(--qrcode-height) !important;
    }

    .show-content {
      display: block !important;
    }
  }

  @page {
    margin: 0;
    size: var(--print-size) !important;
  }
}

.home {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 100px;

  .title {
    font-weight: 900;
    font-size: 24px;
  }

  .input {
    width: 200px;
    margin-bottom: 30px;
    resize: both;
  }

  .size {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 30px;

    &-title {
      color: palevioletred;
      font-weight: 700;
      font-size: 16px;
    }

    &-input {
      margin-bottom: 5px;
    }
  }

  .hide-content {
    display: none;
    font-size: var(--content-size);
  }

  .qrcode {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 250px;
    height: 250px;
    color: darkslateblue;
    font-weight: 700;
    font-size: 16px;
    background: lightblue;
  }

  .btn-wrapper {
    display: flex;
    align-items: center;

    .btn {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 200px;
      height: 50px;
      margin: 0 10px;
      margin-top: 30px;
      border-radius: 5px;
      color: rgb(70, 70, 70);
      font-weight: 900;
      font-size: 24px;
      background: darkgrey;

      &:hover {
        cursor: pointer;
        opacity: 0.8;
      }

      &:active {
        opacity: 0.6;
      }
    }
  }
}
</style>
