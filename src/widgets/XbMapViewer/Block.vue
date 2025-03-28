<script setup lang="ts">
import { computed, onMounted, ref } from "vue";

import { getImagePath } from "@/utils/utils";

import { TipMap, bgMap, classMap } from "./consts";

const props = withDefaults(
  defineProps<{
    tile: string;
    tileHeightType: string;
    tokens?: string[];
    black?: string;
  }>(),
  {
    black: "",
  },
);

const self = ref();
const style = computed(() => {
  const result = {} as Record<string, string>;
  if (bgMap[props.tile!])
    result.backgroundImage = `url(${getImagePath(bgMap[props.tile!])})`;

  for (const token of props.tokens || []) {
    if (token && bgMap[token])
      result.backgroundImage = `url(${getImagePath(bgMap[token])})`;
  }

  if (
    props.tile === "grass" &&
    (props.tileHeightType === "1" || props.tileHeightType === "HIGHLAND")
  ) {
    result.boxShadow = "5px 5px 8px black";
    result.filter = "brightness(1.15)";
    result.zIndex = "2";
  }

  return result;
});

onMounted(() => {
  let content = "";
  if (props.tile && TipMap[props.tile]) content += TipMap[props.tile];

  for (const token of props.tokens || []) {
    if (token && TipMap[token])
      content += content ? `<br/> ${TipMap[token]}` : ` ${TipMap[token]}`;
  }

  if (content.length > 0) {
    // @ts-expect-error tippy

    tippy6(self.value, {
      allowHTML: true,
      content,
      arrow: true,
      theme: "light-border",
      size: "large",
      maxWidth: 250,
    });
  }
});
</script>

<template>
  <div
    ref="self"
    :class="[
      'block',
      tile,
      tokens ? `token ${tokens.toString().replace(/,/g, ' ')}` : '',
      black,
    ]"
    :style="style"
  >
    <template v-if="tokens">
      <template v-for="(token, i) in tokens" :key="i">
        <span v-if="classMap[token!]">
          <i :class="classMap[token!]" />
        </span>
      </template>
    </template>
    <template v-else>
      <span v-if="classMap[tile!]">
        <i :class="classMap[tile!]" />
      </span>
    </template>
  </div>
</template>

<style scoped>
.block {
  display: inline-block;
  position: relative;
  background-color: gray;
}

@media screen and (max-width: 540px) {
  .block {
    border-width: 1px !important;
    border-style: none;
    border-color: transparent;
  }
}

.block:before {
  content: "";
  display: block;
  padding-top: 100%;
}

.block span {
  position: absolute;
  /*font-weight: bold;*/
  font-family: initial;
  color: white;
  pointer-events: none;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.token {
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
}

.rect {
  filter: opacity(0.75);
}

.empty {
  background-color: transparent;
}

.start,
.flystart {
  background-color: indianred;
  border: 2px solid darkred;
}

.start span,
.flystart span,
.ballis span,
.ore span,
.airsup span,
.xbalis span,
.xbbillb span {
  color: darkred;
}

.end {
  background-color: skyblue;
  border: 2px solid dodgerblue;
}

.end span {
  color: dodgerblue;
}

.floor {
  background-color: darkgray;
  border: 1px dashed yellow;
  box-shadow: inset 0 0 0 2px black;
}

.floor span {
  color: white;
}

.road {
  background-color: darkgray;
  border: 2px dotted dimgray;
}

.wall {
  background-color: lightgray;
  border: 2px dotted dimgray;
  box-shadow: 5px 5px 8px black;
  z-index: 2;
}

.fence_bound {
  background-color: darkgray;
  border: 2px solid lightgray;
}

.hole {
  background-color: black;
}

.grass {
  background-color: yellowgreen;
  border: 2px dotted dimgray;
}

.deepsea,
.xbdpsea {
  /* 清澈水域，为啥有两个得问YJ */
  background-color: steelblue;
  border: 2px dotted darkgray;
}

.infection,
.corrosion_2,
.healing,
.telin,
.telout,
.volcano,
.volspread {
  background-color: darkgray;
  border: 2px dotted dimgray;
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
}

.corrosion_2 span {
  color: darkgoldenrod;
}

.telin,
.telout {
  border: 2px dashed dimgray;
}

.telin {
  transform: rotate(180deg); /* fa 和 windicss 都不起作用 */
}

.token {
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
}

.hiddenstone {
  /* 遗迹残骸 */
  background-color: dimgray;
  border: 2px solid darkgray;
}

.streasure span {
  /* 宝刺金属箱 */
  color: black;
}

.roadblock {
  /* 道路障碍物 */
  background-color: slategray;
  border: 2px dotted darkgray;
}

.hstone span {
  /* 城防路障 */
  color: #745d32;
}

.xbmgbird span {
  /* 喙中奇物 */
  color: #653409;
}

.airsup {
  /* 空袭侵入点 */
  background-color: indianred;
  border: 2px solid darkred;
}

.wdescp span, /* 逃脱点 */
.xbbase span, /* 基地 */
.xbfdtion span, /* 风沙营垒 */
.xbmcv span /* 矿业房车 */ {
  color: green;
}

.tower span {
  /* 留声机 */
  color: darkslategray;
}

.redtower span /* 移动战塔 */
.xbstation span /* 号令点 */ {
  color: crimson;
}

/* 池沼地块 */
.puddle {
  background-color: #599bd1;
  border: 2px dotted darkgray;
}
.puddle span {
  color: #82bded;
}

/* 隐焰地块 */
.steam {
  background-color: #935858;
  border: 2px dotted darkgray;
}
.steam span {
  color: #973a3a;
}

.xbrandprop span {
  color: rgb(255, 229, 194);
}

.xbcp span {
  color: rgb(255, 255, 99);
}
</style>
