

ref: [Continuous Profiling Definition - CNCF](https://www.cncf.io/blog/2022/05/31/what-is-continuous-profiling/)

## Why Continuous Profiling 
* performance monitoring
	* To understand resource usage at any particular time
* resource utilization, frequency, duration of function calls
	* e.g. locate which part of the application are consuming the most resources
	
### Business case
ref: [Continuous profiling with Grafana Pyroscope: developer experience, flame graphs, and more](https://grafana.com/about/events/grafanacon/2023/session/continuous-profiling-with-grafana-pyroscope/)
![[Pasted image 20230801174458.png]]
![[Pasted image 20230801181001.png]]
#### Continuous Profiling & Traces/Metrics/Logs
![[Pasted image 20230801180357.png]]

#### Pioneers who do Continuous Profiling 
- Netflix
- Google
## Continuous Profiling Tools

### [Pyroscope](https://pyroscope.io/)

> Grafana Phlare and Pyroscope are merging into Grafana Pyroscope.

ref: https://github.com/grafana/pyroscope

#### Usage
-   Find performance issues and bottlenecks in your code
-   Use high-cardinality tags/labels to analyze your application
-   Resolve issues with high CPU utilization
-   Track down memory leaks
	- https://grafana.com/blog/2023/04/19/how-to-troubleshoot-memory-leaks-in-go-with-grafana-pyroscope/
-   Understand the call tree of your application
-   Auto-instrument your code to link profiling data to traces
ref: https://github.com/grafana/pyroscope

#### profiling data format
- rbspy(ruby)
- eBPF(Linux Kernel)

#### Flame graphs
visualization of profiles
https://flamegraph.com/

#### Implementation
- Pyroscope SDKs(Push)
- Grafana Agent(Pull)
- eBPF(get whole data from cluster without instrumenting every single application)
![[Pasted image 20230801175458.png]]

## Questions
- How Continuous Profiling tool works with open telemetry?
- eBPF?
- Continuous Profiling & Green 
- Continuous Profiling & FinOps



