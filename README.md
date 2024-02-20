## 「Vue3 フロントエンド開発の教科書」13章
## 13.1
### 13.1.1
```ts
vueChapter13.1/utest-basic/src/App.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';
  import {divideTwoNums} from "@/functions";

  const num1 = ref(6);
  const num2 = ref(3);
  const ans = computed(
    (): number => {
      return divideTwoNums(num1.value, num2.value);
    }
  );
</script>

<template>
  <p>{{ num1 }}÷{{ num2 }}={{ ans }}</p>
</template>
```
<br>

```ts
vueChapter13.1/utest-basic/src/functions.ts

export function divideTwoNums(num1: number, num2: number): number {
    return num1 / num2;
}
```
<br>

```ts
vueChapter13.1/utest-basic/src/_tests__/App.spec.ts

import { describe, test, expect } from "vitest";
import {mount} from "@vue/test-utils";
import App from "@/App.vue";

describe(
    "App.vueのテスト",
    () => {
        test(
            "初期値での表示内容テスト",
            () => {
                const wrapper = mount(App);
                const actual = wrapper.get("p").text();
                const expected = "6÷3=2";
                expect(actual).toBe(expected);
            }
        );
    }
);
```
<br>

```ts
vueChapter13.1/utest-basic/src/_tests__/functions.spec.ts

import { describe, test, expect } from "vitest";
import { divideTwoNums } from "@/functions";

describe(
    "functions.tsのdivideTwoNums()関数のテスト",
    () => {
        test(
            "単純な割り算の場合のテスト",
            () => {
                const num1 = 6;
                const num2 = 3;
                const actual = divideTwoNums(num1, num2);
                const expected = 2;
                expect(actual).toBe(expected);
            }
        );
        test(
            "分子が0の場合のテスト",
            () => {
                const num1 = 0;
                const num2 = 3;
                const actual = divideTwoNums(num1, num2);
                const expected = 0;
                expect(actual).toBe(expected);
            }
        );
        test(
            "分母が0の場合のテスト",
            () => {
                const num1 = 6;
                const num2 = 0;
                const actual = divideTwoNums(num1, num2);
                const expected = Infinity;
                expect(actual).toBe(expected);
            }
        );
    }
);
```
<br>

## 13.3
### 13.3.1
```ts
vueChapter13.3/utest-input/src/App.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';

  const num1 = ref(6);
  const num2 = ref(3);
  const ans = computed(
    (): number => {
      return num1.value / num2.value;
    }
  );
</script>

<template>
  <p>
    <input type="number" data-testid="num1" v-model="num1">
    ÷
    <input type="number" data-testid="num2" v-model="num2">
    = <span data-testid="ans">{{ ans }}</span>
  </p>
</template>
```
<br>

```ts
vueChapter13.3/utest-input/src/__tests__/App.spec.ts

import { describe, test, expect } from "vitest";
import {mount} from "@vue/test-utils";
import App from "@/App.vue";
import { wrap } from "module";

describe(
    "App.vueのテスト",
    () => {
        test(
            "初期値の場合のテスト",
            () => {
                const wrapper = mount(App);
                const actual = wrapper.get(`[data-testid="ans"]`).text();
                const expected = "2";
                expect(actual).toBe(expected);
            }
        );
        test(
            "分子8で分母2を入力した場合のテスト",
            async () => {
                const wrapper = mount(App);
                await wrapper.get(`[data-testid="num1"]`).setValue(8);
                await wrapper.get(`[data-testid="num2"]`).setValue(2);
                const actual = wrapper.get(`[data-testid="ans"]`).text();
                const expected = "4";
                expect(actual).toBe(expected);
            }
        );
    }
);
```
<br>

```ts
vueChapter13.3/utest-trigger/src/App.vue

<script setup lang="ts">
  import { ref } from 'vue';

  const isVisible = ref(false);
  const onShowButtonClick = (): void => {
    isVisible.value = true;
  };
</script>

<template>
  <p>
    隠し領域を
    <button data-testid="showButton" v-on:click="onShowButtonClick">表示</button>
  </p>
  <p v-if="isVisible" data-testid="invisible">表示されました</p>
</template>
```
<br>

```ts
vueChapter13.3/utest-trigger/src/__tesets__/App.spec.ts

import { describe, test, expect } from "vitest";
import {mount} from "@vue/test-utils";
import App from "@/App.vue";

describe(
    "App.vueのテスト",
    () => {
        test(
            "初期状態(隠し領域非表示)のテスト",
            () => {
                const wrapper = mount(App);
                const actual = wrapper.find(`[data-testid="invisible"]`).exists();
                const expected = false;
                expect(actual).toBe(expected);
            }
        );
        test(
            "表示ボタンをクリックした場合のテスト",
            async () => {
                const wrapper = mount(App);
                await wrapper.get(`[data-testid="showButton"]`).trigger("click");
                const actual = wrapper.find(`[data-testid="invisible"]`).exists();
                const expected = true;
                expect(actual).toBe(expected);
            }
        );
    }
);
```
<br>

## 13.4
### 13.4.1
```ts
vueChapter13.4/utest-components/src/App.vue

<script setup lang="ts">
  import { ref, computed } from 'vue';
  import OneMember from './components/OneMember.vue';

  const memberListInit = new Map<number, Member>();
  memberListInit.set(33456, {id: 33456, name: "田中太郎", email: "bow@example.com", points: 35,
  note: "初回入会特典あり"});
  memberListInit.set(47783, {id: 47783, name: "鈴木二郎", email: "mue@example.com", points: 53});
  const memberList = ref(memberListInit);

  const totalPoints = computed(
    (): number => {
      let total = 0;
      for(const member of memberList.value.values()) {
        total += member.points;
        }
        return total;
      }
    );
  const onIncrementPoint = (id: number): void => {
    const member = memberList.value.get(id);
    if(member != undefined) {
      member.points++;
      }
    }
  
  interface Member {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
    }
</script>

<template>
  <section>
    <h1>会員リスト</h1>
    <p>全会員の保有ポイントの合計: {{ totalPoints }}</p>
    <OneMember
      v-for="[id, member] in memberList"
      v-bind:key="id"
      v-bind:id="id"
      v-bind:name="member.name"
      v-bind:email="member.email"
      v-bind:points="member.points"
      v-bind:note="member.note"
      v-on:incrementPoint="onIncrementPoint"/>
  </section>
</template>
```
<br>

```ts
vueChapter13.4/utest-components/src/__tests__/App.spec.ts

import { describe, test, expect } from "vitest";
import { mount } from "@vue/test-utils";
import App from "@/App.vue";
import OneMember from "@/components/OneMember.vue";

describe(
    "App.vueのテスト",
    () => {
        test(
            "合計点数表示のテスト",
            () => {
                const options = {
                    global: {
                        stubs: {
                            OneMember: true
                        }
                    }
                };
                const wrapper = mount(App, options);
                const actualText = wrapper.text();
                const expected = "全会員の保有ポイントの合計: 88";
                expect(actualText).toContain(expected);
            }
        );
        test(
            "Emitメソッドのテスト",
            async () => {
                const options = {
                    global: {
                        stubs: {
                            OneMember: true
                        }
                    }
                };
                const wrapper = mount(App, options);
                const oneMemberComponent = wrapper.findComponent(OneMember);
                await oneMemberComponent.vm.$emit("incrementPoint", 33456);
                const actualText = wrapper.text();
                const expected = "全会員の保有ポイントの合計: 89";
                expect(actualText).toContain(expected);
            }
        );
    }
);
```
<br>

```ts
vueChapter13.4/utest-components/src/components/OneMember.vue

<script setup lang="ts">
  import { computed } from 'vue';

  interface Props {
    id: number;
    name: string;
    email: string;
    points: number;
    note?: string;
    }

    interface Emits {
      (event: "incrementPoint", id: number): void;
      }

  const props = defineProps<Props>();
  const emit = defineEmits<Emits>();

  const localNote = computed(
    (): string => {
      let localNote = props.note;
      if(localNote == undefined) {
        localNote = "--";
        }
        return localNote;
      }
    );
  const pointUp = (): void => {
    emit("incrementPoint", props.id);
    }
</script>

<template>
  <section class="box">
    <h4>{{ name }}さんの情報</h4>
    <dl>
      <dt>ID</dt>
      <dd>{{ id }}</dd>
      <dt>メールアドレス</dt>
      <dd>{{ email }}</dd>
      <dt>保有ポイント</dt>
      <dd>{{ points }}</dd>
      <dt>備考</dt>
      <dd>{{ localNote }}</dd>
    </dl>
    <button v-on:click="pointUp">ポイント加算</button>
  </section>
</template>

<style scoped>
  .box {
    border: green 1px solid;
    margin: 10px;
  }
</style>
```
<br>

```ts
vueChapter13.4/utest-components/src/components/OneMember.spec.ts

import { describe, test, expect } from "vitest";
import {mount} from "@vue/test-utils";
import OneMember from "@/components/OneMember.vue";

describe(
    "OneMember.vueのテスト",
    () => {
        test(
            "備考データがある場合のPropsのテスト",
            () => {
                const propsData = {
                    id: 22458,
                    name: "中野三郎",
                    email: "mue@wow.com",
                    points: 200,
                    note: "すばらしい！"
                };
                const wrapper = mount(OneMember, {props: propsData});
                const actualText = wrapper.text();
                expect(actualText).toContain(String(propsData.id));
                expect(actualText).toContain(propsData.name);
                expect(actualText).toContain(propsData.email);
                expect(actualText).toContain(String(propsData.points));
                expect(actualText).toContain(propsData.note);
            }
        );
        test(
            "備考データがない場合のPropsのテキスト",
            () => {
                const propsData = {
                    id: 55148,
                    name: "江口四郎",
                    email: "egu@chi.com",
                    points: 300
                };
                const wrapper = mount(OneMember, {props: propsData});
                const actualText = wrapper.text();
                expect(actualText).toContain(String(propsData.id));
                expect(actualText).toContain(propsData.name);
                expect(actualText).toContain(propsData.email);
                expect(actualText).toContain(String(propsData.points));
                expect(actualText).toContain("--");
            }
        );
        test(
            "[ポイント加算]ボタンクリックのテスト(Emitのテスト)",
            async () => {
                const propsData = {
                    id: 22458,
                    name: "中野三郎",
                    email: "mue@wow.com",
                    points: 200,
                    note: "すばらしい！"
                };
                const wrapper = mount(OneMember, {props: propsData});
                await wrapper.get("button").trigger("click");
                const incrementPointEvent = wrapper.emitted("incrementPoint");
                expect(incrementPointEvent).toHaveLength(1);
                const expectedIncrementPointEvent = [[propsData.id]];
                expect(incrementPointEvent).toEqual(expectedIncrementPointEvent);
            }
        );
    }
);
```