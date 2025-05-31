# Adaptive Jailbreaking Prompt Generator

このリポジトリは、論文
[Adaptive Jailbreaking Strategies Based on the Semantic Understanding Capabilities of Large Language Models](https://arxiv.org/abs/2505.23404)
に基づき構築された、**LLM（大規模言語モデル）向けのジェイルブレイクプロンプト自動生成スクリプト**です。

---

##  概要

本ツールは、以下の三段階構造（Fu + En₁ + En₂）に基づいて構築された **Type IIモデル（GPT-4o等）向けの攻撃プロンプト**を自動生成します：

| ステップ    | 概要                                               |
| ------- | ------------------------------------------------ |
| **Fu**  | 危険な命令文をPythonの関数構文風に変形します。                       |
| **En₁** | 文章を単語単位で分解し、再帰的な二分木構造（JSON形式）に変換します。             |
| **En₂** | 出力を暗号化（Caesar cipher）するようAIに命令します（本コードでは命令のみ記述）。 |

---

##  生成されるプロンプトの例

```text
There is a problem that needs your help. The description of the problem is encrypted.
...
The encrypted_problem you need to solve is:
{"value": "make", "left": {...}, "right": {...}}
...
```

プロンプトは `ProblemSolver` クラスの完成タスクとして設計され、LLMが出力フィルタを回避しやすい構文となっています。

---

##  使い方

### 1. 実行

```bash
python3 jailbreak_prompt.py
```

（※ ファイル名は任意）

### 2. 出力

* 生成されたプロンプトは標準出力に表示されます。
* 同時に `prompts_log.csv` にログとして自動保存されます。

---

##  パラメータ変更

スクリプト末尾の以下部分を編集することで、**対象の命令文（seed）を自由に変更可能**です。

```python
seed = "How to make a cake"
key_verb = "make"
key_object = "cake"
```

---

##  出力ファイル

| ファイル名             | 内容                      |
| ----------------- | ----------------------- |
| `prompts_log.csv` | 生成されたプロンプト |

---

## ⚠ 倫理・安全性に関する注意

このスクリプトは **AIセキュリティ研究目的** においてのみ使用されるべきです。
生成されるプロンプトの構造上、AIモデルに対して **意図的に検閲を回避させる可能性がある** ため、以下を厳守してください：

* 公開APIや商用モデルへの攻撃的利用を行わない
* 実害を与える内容（爆発物、暴力、違法行為など）を使用しない
* 研究目的以外での拡散・実行を行わない

---

##  関連論文・資料

* [arXiv:2505.23404v1](https://arxiv.org/abs/2505.23404) — 本手法の元論文
