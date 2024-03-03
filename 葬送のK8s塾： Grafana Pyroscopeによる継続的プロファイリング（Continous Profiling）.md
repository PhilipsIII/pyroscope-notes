## 表紙

---
## 活動概要

- プロファイリングと継続的プロファイリングの理解 🌞
- Grafana Pyroscopeの計装方法の実装 🌥
---
## プロファイリングと継続的プロファイリング
---
### プロファイリング
> プロファイリングとはコードの動的分析の一種です。アプリケーションの実行時にその特性をキャプチャし、この情報を使用してアプリケーションの速度と効率を改善する方法を見つけます。

---

> これまで、プロファイリングはア**プリケーション開発時にのみ**実行されていました。このアプローチでは、本番環境を正確に予測できる負荷テストとベンチマークを開発できなければなりませんでした。

https://cloud.google.com/profiler/docs/concepts-profiling

---
#### 本番環境でのプロファイリングが滅多にやらない理由
- よいツールがなかった
	- データ取得の自動化ができない
	- 計測による本番システムの性能に与えてしまう影響が大きい
- 本番環境への接続を要するための権限と責任問題
- 当時定義した性能要件通り作って試験が通れば、本番もそのまま動ける迷信 

---

### 継続的プロファイリング
> 継続的プロファイリングとは、**本番環境**で実行中のアプリケーションをプロファイリングすることです。このアプローチにより、本番環境の正確な予測負荷テストとベンチマークの開発が不要になります。継続的なプロファイリングに関する調査で、その高い正確性と費用対効果が証明されています。

https://cloud.google.com/profiler/docs/concepts-profiling

---

#### 利点
![](https://grafana.com/static/img/pyroscope/profiling-use-cases-diagram.png)

---
## 継続的プロファイリングの実践（ツール編）
---
### 予備知識
#### プロファイリングデータ(Profiling data)
- CPU使用率：　使用率が高い場合、コードが非効率の可能性がある
- メモリ（Memory Allocation）：　時間軸でメモリ割り当てが増加する一方の場合、メモリリークの可能性がある
- ...
---
#### データの取得：プロファイラー(Profiler)
- ターゲットからプロファイリングデータを収集するプログラム
- サンプリング方式が主流になった。（ターゲットの性能に与える影響が小さい）
- Java: [Flight Recorder](https://docs.oracle.com/javacomponents/jmc-5-4/jfr-runtime-guide/about.htm#JFRUH170), [VisualVM (OSS)](https://visualvm.github.io/download.html), [JProfiler (commercial)](https://www.ej-technologies.com/products/jprofiler/overview.html)
- Golang: [pprof](https://github.com/google/pprof)
---
#### データの可視化：フレームグラフ(Flamegraph)🔥 
---
> 「我が夢は、すべての性能を完全に理解することだ！」 -- Brendan Gregg, Flamegraph 発明者 & *詳解 システム・パフォーマンス* 著者

![[Brendan Gregg名言.png|500]]

---

![|500](https://grafana.com/static/img/pyroscope/code-to-flamegraph-animation.gif)

---
- Tools provided by Cloud Vendors
	- [GCP Cloud Profiler](https://cloud.google.com/profiler/docs/about-profiler) 
	- [Azure Monitor Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview) 
	- [AWS CodeGuru Profiler](https://docs.aws.amazon.com/codeguru/latest/profiler-ug/what-is-codeguru-profiler.html) 
	- ...
- Tools provided by O11y tool vendors
	- Datadog: https://docs.datadoghq.com/profiler/
	- Elastic: https://www.elastic.co/observability/universal-profiling
	- ...
---
- Tools provided by OSS
![|500](https://user-images.githubusercontent.com/23323466/192879029-f10378ca-1b88-4441-9cf4-ba24b6f62256.png)
---

#### [Grafana Pyroscope](https://github.com/grafana/pyroscope)
![|500](https://private-user-images.githubusercontent.com/662636/263812497-c1fc4055-b33d-4e69-a450-9e7a7b2317bb.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDk0Njc0MTMsIm5iZiI6MTcwOTQ2NzExMywicGF0aCI6Ii82NjI2MzYvMjYzODEyNDk3LWMxZmM0MDU1LWIzM2QtNGU2OS1hNDUwLTllN2E3YjIzMTdiYi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwMzAzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDMwM1QxMTU4MzNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hOTE4ZWMwZGE4MWU1NTRmMTM2NmE1YmI3OTFmOWQwMWI5M2E2ZmQ1Y2ZkNDk3ZGNkMTljNDc0ZjMyODAyY2FlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.-lTOtVmiDqFdxEN4n7xJNxS4gbHqZpqihPfrIuHXuN4)

- 2023年3月Grafana LabがPyroscopeを買収し、Grafana Phlareと統合した。


---
##### デプロイメント方法

| モノリシックモード                                                                                                                 | マイクロサービスモード                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| ![\|400](https://grafana.com/docs/pyroscope/latest/reference-pyroscope-architecture/deployment-modes/monolithic-mode.svg) | ![\|400](https://grafana.com/docs/pyroscope/latest/reference-pyroscope-architecture/deployment-modes/microservices-mode.svg) |


---

| モノリシックモード                  | マイクロサービスモード                                       |
| -------------------------- | ------------------------------------------------- |
| 全てのコンポーネントが一つのプロセスにデプロイされる | コンポーネントは個別のプロセスにデプロイされる                           |
| シンプル                       | コンポーネントごとのスケーリングがしやすい割に、<br>アーキテクチャが複雑になり、メンテしにくい |

---

##### 計装方法
- SDK（Pushモード）：　プログラム改修が必要
- Grafana Agent（Scrape/Pullモード）：　プログラム改修が不要
![|500](https://grafana.com/media/docs/pyroscope/pyroscope_client_server_diagram.png)

---



---
### Grafana Pyroscope Demo
- モノリシックモード 
- Grafana Agent
- 公式サンプルアプリ

---

(ちょっと簡単すぎない？！)
![|300](https://i.pinimg.com/564x/74/c7/00/74c7009362d43b85b1e64cc71c80d304.jpg)

---

所感：公式手順と構築資材が日々進化している
![|300](https://pbs.twimg.com/media/F7P-Jt2bUAAkhir.jpg)


---

> Prometheusでも同じ運用課題はよくありますが、特にGrafana Agentの場合はさらに情報が少なく、公式ドキュメントに記載されてないことが多々あるので、Prometheusを元々触ってた人でないと今は設定ファイルを書くのがかなり難しいです。

[# Grafana Agent を用いた Continuous Profiling (golang pull編) @yosshi](https://qiita.com/yosshi_/items/5d2b917027787b9a9f1e)

---
## Future Trends
---
### Profiling 💖 OTel
- Profiling as OTel's fourth signal
	- [Data model merged](https://github.com/open-telemetry/oteps/pull/239)
	- Next steps: Collector implementation, API Specification, SDK Specification, ...
	- [Elastic's Continuous Profiling Agent Donation Proposal](https://github.com/open-telemetry/community/issues/1918)
- [For more info: OTel Profiling SIG Meeting Notes](https://docs.google.com/document/d/19UqPPPlGE83N37MhS93uRlxsP1_wGxQ33Qv6CDHaEp0/edit)
---
### Profiling 💖 eBPF 
![|200](https://ebpf.io/static/logo-black-98b7a1413b4a74ed961d292cf83da82e.svg)
- 拡張BPF（eBPF）はLinuxカーネルレイヤーで軽量なサンドボックスされた仮想マシンであり、カーネルのソースコードを変更せずに、本来ユーザスペースでしか実装できないプログラムを効率的に実行できる技術。
- プロファイリングの場合、システム全体のスタックトレースを一定のレート (100Hz など) で取得するプログラムを実行することを意味します。
---
#### 言語ごとに使い分け

| 言語タイプ | コンパイル型言語(e.g. Golang, Java)       | インタプリタ型言語(e.g. Python, Ruby)               |
| ----- | --------------------------------- | ------------------------------------------ |
| 推奨    | eBPFプロファイラー                       | 非eBPFプロファイラー                               |
| 理由    | 非eBPFプロファイラーと似ている情報が取得できる         | ランタイム内のスタックトレースにカーネルから簡単にアクセスできないため        |
| メリット  | ソースコードを修正せずプロファイリングデータが取得できる      | 既存のプロファイラー技術は成熟している                        |
| デメリット | 強い権限を与える必要がある；<br>まだCPU情報しか取得できない | インフラストラクチャのメタデータ (i.e. k8s) に自動タグ付けする機能の制約 |

---
- https://grafana.com/docs/pyroscope/latest/configure-client/grafana-agent/ebpf/
	- eBPF to profile the node(e.g. k8s cluster) 
	- specific language instrumentation per application
---
### Profiling 💖 AI
- AI-Powered Flamegraph Interpreter in Grafana Pyroscope
https://pyroscope.io/blog/ai-powered-flamegraph-interpreter/

---
## Off-Topic
---
### [Efficient Go Japanese release Feb.24th ](https://www.oreilly.co.jp/books/9784814400539/)
![|500](https://www.oreilly.co.jp/books/images/picture_large978-4-8144-0053-9.jpeg)

---
### 効率化フェーズにおけるO11y
![[Efficiency-aware development flow(Efficiency phase focused).png|500]]

---
### (Who still needs to read this?) 
![[juku_off_topic_meme.JPG|500]]

---

## Other References
- https://www.infoq.com/articles/open-source-java-profilers/
- https://grafana.com/docs/pyroscope/latest/introduction/flamegraphs/
- [Continuous Profiling is proposed by Google, 1997](https://dl.acm.org/doi/10.1145/265924.265925)
- [Continuous Profiling in production by Google, 2010](https://research.google/pubs/google-wide-profiling-a-continuous-profiling-infrastructure-for-data-centers/)
- https://yowcon.com/sydney-2022/sessions/2394/visualizing-performance-the-developers-guide-to-flame-graphs
- https://pyroscope.io/blog/ebpf-profiling-pros-cons/
- https://qiita.com/yosshi_/items/ab8252c1b44533c5e1b6
- https://grafana.com/docs/pyroscope/latest/view-and-analyze-profile-data/profiling-types/
- https://grafana.com/blog/2023/03/15/pyroscope-grafana-phlare-join-for-oss-continuous-profiling/
- https://brendangregg.com/index.html
