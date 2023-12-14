## Ref.
- https://hackernoon.com/an-intro-to-continuous-profiling-in-kubernetes-via-pyroscope
	- https://github.com/infracloudio/microservices-demo-dev?ref=hackernoon.com
- https://grafana.com/blog/2023/04/19/how-to-troubleshoot-memory-leaks-in-go-with-grafana-pyroscope/
- [Grafana Agent を用いた Continuous Profiling (golang pull編)](https://qiita.com/yosshi_/items/5d2b917027787b9a9f1e)
- [Grafana Agent を用いた Continuous Profiling (eBPF編)](https://qiita.com/yosshi_/items/ab8252c1b44533c5e1b6)
## Sample Application
### Google microservices-demo❓
- https://github.com/GoogleCloudPlatform/microservices-demo
	- Grafana Agent - Go pull mode
		- pprofエンドポイントをexposeしなければならないようで
		- （それでしたら、SDKを使うGo Push modeとは大した手間を減らせないでは？）
	- Grafana Agent - eBPF mode
	- （時間があればトライ）Cloud Profilerで継続プロファイリング：
		- https://qiita.com/yoppe/items/c8fbeea6e1f416f9bf18
		- https://cloud.google.com/profiler/docs?hl=ja

### Pyroscope + Grafana Agent(Go pull mode) demo ✅
- https://github.com/grafana/pyroscope/blob/main/examples/golang-pull/kubernetes/README.md
	- To be able to pull profiles from applications, your applications needs to expose [pprof endpoints](https://pkg.go.dev/net/http/pprof).
	- 
![[Profiling sample application - grafana agent go pull mode.png]]