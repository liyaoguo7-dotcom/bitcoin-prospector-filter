# BitcoinProspector 正式过滤器

给 [bitcoin-prospector-fast](https://github.com/liyaoguo7-dotcom/bitcoin-prospector-fast) 用的
有钱地址过滤器：布隆过滤器 + 两个精确名单。

来源：比特币 UTXO 快照 `utxo-937817.dat.zst`（区块高度 937817，2026 年 2 月），
164,547,146 条 UTXO，去重后 54,485,309 个有钱地址：
legacy（P2PKH / P2SH / P2WPKH 的 hash160）49,706,018 个，
taproot（P2TR 的 x 坐标）4,779,291 个。

## 下载

四个文件在 Releases 里（四个附件，一共约 1.71 GB），下下来放进同一个目录：

| 文件 | 大小 | SHA256 |
| ---- | ---- | ------ |
| `filter.bin` | 130,560,486 字节（124.5 MB） | `7237213ee58ee8acba02c2db39a896da5c72ba1529c70bd69ec70a861ff55164` |
| `legacy.bin` | 1,391,768,504 字节（1.33 GB） | `6f1b4250a5008c44af501345d75fb74748279f632db17575dcb92e9ec3ddd0dc` |
| `taproot.bin` | 191,171,640 字节（182 MB） | `107a7de60c2cecf93d6aa21a48937ee30d1b332d5d8fecf07e7b1c30dbf10c64` |
| `manifest.json` | 685 字节 | （本仓库里也有一份，含三文件的 SHA256 和生成时间） |

下载完先对 SHA256，`filter.bin` 和两个精确名单缺一个都跑不起来。

## 注意

- 快照是 2026 年 2 月的，旧了 9 个月：之后新出现有钱的地址查不到，
  之后已经花掉的地址还留在过滤器里。命中了先去网上核对再说。
- 命中了怎么办：只写服务器本地 `found.txt`，不外传私钥；
  **撞上有余额的地址，币绝不能动，那是别人的。**
- P2TR 行的私钥是 tweaked 之后的可花钥匙，不是原始私钥。
- 构建方法（解析、建过滤器、验证）在代码仓库的 `tools/` 里：
  `parse-snapshot.mjs`、`build-filter-streaming.mjs`、`verify-filter.mjs`。
