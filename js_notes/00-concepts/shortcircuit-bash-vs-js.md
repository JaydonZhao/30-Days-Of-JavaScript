# 通用概念 · `&&` / `||`:Linux(bash) vs JavaScript

> 跨环境对照。shell 和 JS 来回切时看这篇。相关:`../05-conditionals/notes.md` 第 5 节。

## 相同点:都是"短路求值"

"左边决定要不要看右边"—— 这个思路两边完全一致。

```bash
# bash
命令A && 命令B    # A 成功才执行 B
命令A || 命令B    # A 失败才执行 B
```
```js
// JS
a && b // a 为真才看 b
a || b // a 为假才看 b
```

## 不同点:操作数是"命令" vs "值"

| | Linux (bash) | JavaScript |
|---|---|---|
| 左右两边是 | **命令**(程序) | **值 / 表达式** |
| "真/假"看什么 | **退出码**:`0`=成功=真,非0=失败=假 | **truthy/falsy**:值本身 |
| 返回什么 | 最后执行命令的退出码 | **操作数原值** |

## ⚠️ 最大的坑:`0` 的真假正好相反

```bash
# Linux:退出码 0 = 成功 = 真
true && echo yes # 打印 yes(true 的退出码是 0)
```
```js
// JS:数字 0 = falsy = 假
0 && console.log('yes') // 不打印(0 是假)
```

- Linux 的 `0`:退出码 0 = "没出错" = **真**
- JS 的 `0`:数值零 = "空/无" = **假**

**同一个 `0`,两个世界真假颠倒。** 这是切换时最容易栽的地方。

## 相同的惯用法(思路可直接迁移)

```bash
# bash:|| 兜底
result=$(cat file) || result="default"
# bash:&& 前置守卫
[ -f config ] && load_config
```
```js
// JS:|| 设默认值
const result = readFile() || 'default'
// JS:&& 守卫
config && loadConfig()
```

## ✅ 一句话

> 短路逻辑 Linux 和 JS **一致**,惯用法也像(`||`兜底、`&&`守卫)。
> 区别:**bash 操作"命令"看退出码,JS 操作"值"看 truthy/falsy**;
> 最大的坑:**`0` 在 bash 是真(成功),在 JS 是假**。
