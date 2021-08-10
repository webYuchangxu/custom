<template>
  <div  class="exportButton">
    <button @click="animationFunc" :class="exportButtonId">
      <div :class="exportAnimationId"></div>
      导出
    </button>
  </div>
</template>
<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
interface Config {
  // eslint-disable-next-line
  handler?: Function;
}
const random = function() {
  // 生成10-12位不等的字符串
  return Number(
    Math.random()
      .toString()
      .substr(2),
  ).toString(36); // 转换成十六进制
};
@Component
export default class PublicServiceExport extends Vue {
  private startPositon!: DOMRectReadOnly;
  private endPositon!: DOMRectReadOnly;
  @Prop({
    type: Object,
    default: () => {
      return {};
    },
  })
  public config!: Config;

  public exportAnimationId = random();
  public exportButtonId = random();
  public delay = 2;
  public async animationFunc() {
    if (this.config.handler) {
      const res = await this.config.handler();
      if (res === "success") {
        this.successFunc();
      }
    }
  }
  private successFunc() {
    const exportAnimationElement = document.getElementsByClassName(
      this.exportAnimationId,
    )[0] as HTMLElement;

    const exportButtonElement = document.getElementsByClassName(
      this.exportButtonId,
    )[0] as HTMLElement;

    this.startPositon = exportButtonElement.getBoundingClientRect();

    this.endPositon = (document.getElementById(
      "export",
    ) as HTMLElement).getBoundingClientRect();

    exportAnimationElement.style.setProperty(
      "--startTop",
      `${this.startPositon.top}px`,
    );

    exportAnimationElement.style.setProperty(
      "--startLeft",
      `${this.startPositon.left}px`,
    );

    exportAnimationElement.style.setProperty(
      "--endTop",
      `${this.endPositon.top}px`,
    );

    exportAnimationElement.style.setProperty(
      "--endLeft",
      `${this.endPositon.left}px`,
    );
    exportAnimationElement.style.setProperty("--delay", `${this.delay}s`);
    exportAnimationElement.classList.add("exportAnimation");
    window.addEventListener("animationend", this.removeExportAnimate);
  }
  public removeExportAnimate(e: Event) {
    const classList = Array.from((e.target as HTMLInputElement)?.classList);
    if (
      classList.filter((item) =>
        [this.exportAnimationId, "exportAnimation"].includes(item),
      ).length === 2
    ) {
      const exportAnimationElement = document.getElementsByClassName(
        this.exportAnimationId,
      )[0] as HTMLElement;
      exportAnimationElement.classList.remove("exportAnimation");
      window.removeEventListener("animationend", this.removeExportAnimate);
    }
  }
}
</script>
<style lang="scss">
.exportButton{
    display: inline-block;
}
.exportAnimation {
  animation: exportAnimation var(--delay) ease 1;
}
.exportAnimation {
  content: url("../assets/excel.png");
  display: block;
  position: fixed;
  width: 25px;
  height: 25px;
}
@keyframes exportAnimation {
  from {
    top: var(--startTop);
    left: var(--startLeft);
  }
  to {
    top: var(--endTop);
    left: var(--endLeft);
  }
}
</style>
