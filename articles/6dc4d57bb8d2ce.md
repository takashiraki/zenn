---
title: "Nuxt / Storybookが使えるようになった？？"
emoji: "📑"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [nuxt, storybook]
published: false
---

# はじめに

普段色々と Nuxt を使うことが多いのですが、前からやりたかったけどちょっと大変そうだなと思ったことがありました。

**そう、『Nuxt で storybook』を使うことです。**

とある時、色々と情報収集をしていると、何やらできるのでは？と思ったので、実際に動くところまで実装していこうと思います！！

### 記事を読むにあたって
この記事は、社会人2年目のヨワヨワプログラマーが書いている記事です。そのため

- 間違った認識
- 間違った説明

などをしている可能性があります。そのようなことを見つけた方は、**優しくご教授いただけますと幸いです。**

それでは、本編に参りましょう。

## 動作環境

下記環境で試しております。
また、Dockerを使って環境構築をしております。

| 環境        | バージョン |
| ----------- | ---------- |
| `node.js`   | 22.15.0    |
| `npm`       | 10.9.2     |
| `Nuxt`      | 3.16.1     |
| `Storybook` | 9.0.0      |

また、ソースコードは下記リポジトリに一通りあります。

https://github.com/takashiraki/nuxt_storybook

## storybookを入れていく
では、さっそくstorybookを入れていこうと思います。今回は、`storybook`の公式に沿って、セットアップをしていこうと思います。

https://storybook.js.org/docs

```bash
npm create storybook@latest
```

そうすると、ヘルプが必要か？的なことが効かれるので、とりあえず助けてほしいのでyesを選択しました。

```bash
/opt/app # npm create storybook@latest
Need to install the following packages:
create-storybook@9.0.0
Ok to proceed? (y) y


> npx
> create-storybook

╭──────────────────────────────────────────────────────╮
│                                                      │
│   Adding Storybook version 9.0.0 to your project..   │
│                                                      │
╰──────────────────────────────────────────────────────╯
? New to Storybook? › - Use arrow-keys. Return to submit.
❯   Yes: Help me with onboarding
    No: Skip onboarding & don't ask again
```

なんとこれだけで、動くようになっているんです！！

![](/images/storybook-1.png)

これだけでもだいぶ嬉しいですね！！

:::message alert
### `Nuxt/Storybook`ではなく、セットアップは`Storybook`公式に沿ってやっていきます。

おそらく最終的に入るものは同じかと思うのですが、Nuxt Storybookの設定方法で行くと、ほっとリロードが効かなかったりと、いまいち動かない部分がありました。そこでいま記事にしている方で試したところ、こちらはうまく動いたので、今回はこの方法を記事にしております。

[Nuxt storybook](https://storybook.nuxtjs.org/getting-started/setup)
:::

## 試しに`shadcn`とかを入れてみる

では、ここで`shadcn`をセットアップして動くか、試してみましょう。

https://www.shadcn-vue.com/docs/installation/nuxt

そして、コンポーネントを一緒に作っていきます。今回はシンプルなボタンにしてみましょう。

```vue:components/Atoms/Buttons/SimpleButton.vue
<script setup lang="ts">
defineProps<{
    icon?: Function
}>();
</script>

<template>
    <Button>
        <component :is="icon" class="w-4 h-4 mr-2"></component> Login with Email
    </Button>
</template>
```

また、これに対する`storybook`も用意してみましょう。

```typescript:components/Atoms/Buttons/SimpleButton.stories.ts
import type { Meta, StoryObj } from "@nuxtjs/storybook";
import SimpleButton from "./SimpleButton.vue";
import { MailOpen } from "lucide-vue-next";

const meta = {
    title: "Atoms/Buttons/SimpleButton",
    component: SimpleButton,
    parameters: {
        layout: "centered",
    },
    tags: ["autodocs"],
} satisfies Meta<typeof SimpleButton>;

export default meta;

type Story = StoryObj<typeof meta>;

export const Default: Story = {
    args: {
        icon: MailOpen,
    },
};
```

これで、一通りは完成になります。

![](/images/storybook-2.png)

## まとめ
今までいろいろな記事を見て、storybookの導入を諦めていました。もちろん個人的な技量もそうですが、ちょっと大変そうだと思っていて、避けていた部分があります。

しかし、ここまで楽になるととっても導入しやすそうな感じですね。

これからも色んな所で使っていきたいなと思っています。