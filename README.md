# ohos-bst-light

`binary-sign-tool` 轻量重写版 —— 基于 `binary-sign-tool` 开源代码逆向分析出二进制自签名算法后，用 C 语言和 Python 各重写一份签名工具实现。

## 用法

```sh
# C 语言版（需编译，支持 gcc 和 clang）
gcc self-sign.c -o self-sign
./self-sign <input_elf> [output_elf] [--force] [--strip]

# Python 版（仅标准库）
python3 self-sign.py <input_elf> [output_elf] [--force] [--strip]
```

参数说明：

| 参数 | 作用 |
|------|------|
| `<input_elf>` | 待签名 ELF 文件。 |
| `[output_elf]` | 输出文件。缺省时原地处理（in-place），签名结果直接写回原文件。|
| `--force` / `-f` | 强制重签。若 ELF 文件已有 .codesign 段（代码签名段），先剥离再重签。 |
| `--strip` | 剥离签名。剥离 ELF 文件中的 .codesign 段（代码签名段）。 |

示例：

```sh
./self-sign mybin                    # 签名（原地处理）
./self-sign mybin mybin.signed       # 签名到新文件
./self-sign --force mybin            # 强制重签
./self-sign --strip mybin            # 剥离签名
```

## 文件

| 文件 | 用途 |
|------|------|
| `self-sign.c` | C 语言实现，自带 SHA-256 + ELF64 section 注入器 + 剥离器，零第三方依赖 |
| `self-sign.py` | Python 实现，仅用标准库 hashlib/struct，零第三方依赖 |

## 相关项目
- [ohos-bst-portable](https://github.com/hqzing/ohos-bst-portable): 剥离官方源码独立编出 `binary-sign-tool`，产物与 OpenHarmony SDK 里集成的 `binary-sign-tool` 同源同质。此项目可以让开发者排除无关组件干扰、专注研究 `binary-sign-tool` 的代码逻辑。
