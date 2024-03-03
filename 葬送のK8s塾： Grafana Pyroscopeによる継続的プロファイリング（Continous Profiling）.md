## 表紙

---
## 活動背景と概要

- 
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
- CPU 
- Memory
- Disk I/O Requests
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
#### Grafana Pyroscope

---
##### 計装方法
![|500](https://grafana.com/media/docs/pyroscope/pyroscope_client_server_diagram.png)

---
##### Deployment modes
- Monolithic mode vs Microservices mode

| ![\|300](https://grafana.com/docs/pyroscope/latest/reference-pyroscope-architecture/deployment-modes/monolithic-mode.svg) | ![\|300](https://grafana.com/docs/pyroscope/latest/reference-pyroscope-architecture/deployment-modes/microservices-mode.svg) |
| ------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |

---

### Grafana Pyroscope Demo


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
### Profiling 💖  ![|200](https://ebpf.io/static/logo-black-98b7a1413b4a74ed961d292cf83da82e.svg)
- 拡張BPF（eBPF）はLinuxカーネルレイヤーで軽量なサンドボックスされた仮想マシンであり、カーネルのソースコードを変更せずに、本来ユーザスペースでしか実装できないプログラムを効率的に実行できる技術。
- 
---
#### 言語ごとに使い分け

| 言語タイプ | コンパイル型言語(e.g. Golang, Java) | インタプリタ型言語(e.g. Python, Ruby) |
| ----- | --------------------------- | ---------------------------- |
| 推奨    | eBPFプロファイラー                 | 非eBPFプロファイラー                 |
| 理由    | 非eBPFプロファイラーと似ている情報が取得できる   |                              |


> When profiling コンパイル型言語, like Golang, the eBPF profiler is able to get very similar information to the non-eBPF profiler.
> 
> With interpreted languages like Ruby or Python, stacktraces in their runtimes are not easily accessible from the kernel. As a result, the eBPF profiler is not able to parse user-space stack traces for interpreted languages.

https://pyroscope.io/blog/ebpf-profiling-pros-cons/

---
- https://grafana.com/docs/pyroscope/latest/configure-client/grafana-agent/ebpf/
	- eBPF to profile the node(e.g. k8s cluster) 
	- specific language instrumentation per application
---
- Elastic Universal Profiling: https://www.elastic.co/observability/universal-profiling
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
- https://pyroscope.io/blog/ebpf-profiling-pros-cons/