- [[#What is profiling|What is profiling]]
	- [[#What is profiling#Why profiling is seldomly practiced in production environment?|Why profiling is seldomly practiced in production environment?]]
- [[#Why Continuous Profiling|Why Continuous Profiling]]
	- [[#Why Continuous Profiling#Business cases|Business cases]]
	- [[#Why Continuous Profiling#Continuous Profiling vs Profiling|Continuous Profiling vs Profiling]]
- [[#How to do Continuous Profiling|How to do Continuous Profiling]]
	- [[#How to do Continuous Profiling#How to do profiling|How to do profiling]]
		- [[#How to do profiling#Profiler|Profiler]]
		- [[#How to do profiling#Flame Graph|Flame Graph]]
	- [[#How to do Continuous Profiling#How to do Continuous Profiling|How to do Continuous Profiling]]
		- [[#How to do Continuous Profiling#Tools provided by Cloud Vendors|Tools provided by Cloud Vendors]]
		- [[#How to do Continuous Profiling#Tools provided by Third party (O11y tool vendors)|Tools provided by Third party (O11y tool vendors)]]
		- [[#How to do Continuous Profiling#Grafana Pyroscope|Grafana Pyroscope]]
	- [[#How to do Continuous Profiling#Grafana Pyroscope Demo|Grafana Pyroscope Demo]]
- [[#Future Trends|Future Trends]]
	- [[#Future Trends#Profiling ❤️ OTel|Profiling ❤️ OTel]]
	- [[#Future Trends#Profiling ❤️ eBPF|Profiling ❤️ eBPF]]
	- [[#Future Trends#Profiling ❤️ AI|Profiling ❤️ AI]]
- [[#Off-Topic|Off-Topic]]
	- [[#Off-Topic#Definition of the term `instrumentation`|Definition of the term `instrumentation`]]
	- [[#Off-Topic#Origin of Continuous Profiling|Origin of Continuous Profiling]]
	- [[#Off-Topic#[Efficient Go Japanese Feb.24th release](https://www.oreilly.co.jp/books/9784814400539/)|Efficient Go Japanese Feb.24th release]]

## Summary of the research activities


---
## What is profiling


> Profiling is a form of dynamic code analysis. You capture characteristics of the application _as it runs_, and then you use this information to identify how to make your application faster and more efficient.

---

> Historically, profiling was performed only during application development. This approach relied on the ability to develop load tests and benchmarks that could accurately predict a production environment.

https://cloud.google.com/profiler/docs/concepts-profiling

---
### Why not profiling in production environment?
- Assumed Production environment = (Performance) Test environment 
- Fear of performance influence in production environment 
- Require access to production
- Manual work, hard to automate

---

## Why Continuous Profiling
---

### Business cases 
![](https://grafana.com/static/img/pyroscope/profiling-use-cases-diagram.png)

---
### Continuous Profiling vs Profiling
---

> _Continuous profiling_ refers to profiling the application while it executes in a production environment. This approach alleviates the need to develop accurate predictive load tests and benchmarks for the production environment. Research on continuous profiling has shown it is accurate and cost-effective.

https://cloud.google.com/profiler/docs/concepts-profiling

- Proactive
- Continuous 

---
## How to do Continuous Profiling
---

### How to do profiling 
#### Profiler
- A program to collect profiling data from the target.
- Instrumenting Profilers vs Sampling Profilers
	- Instrumenting Profilers influence performance badly.
	- Modern profilers nowadays are mostly sampling profilers.
---
- Java: use Java Management console(JMX) to connect to a running JVM and collects and displays key characteristics in real time.
	- [Flight Recorder](https://docs.oracle.com/javacomponents/jmc-5-4/jfr-runtime-guide/about.htm#JFRUH170)
	- [VisualVM (OSS)](https://visualvm.github.io/download.html)
	- [JProfiler (commercial)](https://www.ej-technologies.com/products/jprofiler/overview.html)
---
- pprof (Golang): https://zenn.dev/muroon/articles/adf577f563c806
#### Flame Graph 
https://grafana.com/docs/pyroscope/latest/introduction/flamegraphs/

---

### How to do Continuous Profiling
---
#### Tools provided by Cloud Vendors
- [GCP Cloud Profiler](https://cloud.google.com/profiler/docs/about-profiler) 
- [Azure Monitor Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview) 
- [AWS CodeGuru Profiler](https://docs.aws.amazon.com/codeguru/latest/profiler-ug/what-is-codeguru-profiler.html) 
---
#### Tools provided by Third party (O11y tool vendors)
- Datadog: https://docs.datadoghq.com/profiler/
- Grafana Pyroscope 
- ...
---
#### Grafana Pyroscope 
- (TODO: How Grafana Pyroscope collect profiling data: architecture graph, mode)
---

---
### Grafana Pyroscope Demo

---
## Future Trends
---
### Profiling ❤️ OTel
- Profiling as OTel's fourth signal
	- [Data model merged](https://github.com/open-telemetry/oteps/pull/239)
	- Next steps: Collector implementation, API Specification, SDK Specification, ...
	- [Elastic's Continuous Profiling Agent Donation Proposal](https://github.com/open-telemetry/community/issues/1918)
- [For more info: OTel Profiling SIG Meeting Notes](https://docs.google.com/document/d/19UqPPPlGE83N37MhS93uRlxsP1_wGxQ33Qv6CDHaEp0/edit)
---
### Profiling ❤️ eBPF

![|200](https://ebpf.io/static/e293240ecccb9d506587571007c36739/f2674/overview.png)

> Allows sandboxed programs to run within the operating system, which means that application developers can run eBPF programs to add additional capabilities to the operating system at runtime.

https://ebpf.io/what-is-ebpf/

---
> When profiling compiled languages, like Golang, the eBPF profiler is able to get very similar information to the non-eBPF profiler.
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
### Profiling ❤️ AI
- AI-Powered Flamegraph Interpreter in Grafana Pyroscope
https://pyroscope.io/blog/ai-powered-flamegraph-interpreter/

---
## Off-Topic
---
### Definition of the term `instrumentation`
- A computer science technique, modifying a program to analyze itself(i.e. measure performance) to diagnose errors and to write trace information.
	- `System.out.println`
---
### Origin of Continuous Profiling 
- [Continuous Profiling is proposed by Google, 1997](https://dl.acm.org/doi/10.1145/265924.265925)
- [Continuous Profiling in production by Google, 2010](https://research.google/pubs/google-wide-profiling-a-continuous-profiling-infrastructure-for-data-centers/)
---
### [Efficient Go Japanese release Feb.24th ](https://www.oreilly.co.jp/books/9784814400539/)
![|500](https://www.oreilly.co.jp/books/images/picture_large978-4-8144-0053-9.jpeg)
---
![[Efficiency-aware development flow(Efficiency phase focused).png|500]]
---
### (Who still needs to read this?) 
![[juku_off_topic_meme.JPG|500]]


## Other References
- https://www.infoq.com/articles/open-source-java-profilers/ 