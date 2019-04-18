# 全局光照性能分析器 (Global Illumination Profiler)

![](../uploads/Main/ProfilerGI.png) 

Global Illumination Profiler 显示实时全局光照 (GI) 子系统的统计信息以及在所有工作线程中消耗的 CPU 时间。GI 由 Unity 中的一个名为 Enlighten 的中间件进行管理。请参阅有关[全局光照](GlobalIllumination.html)的文档以了解更多信息。

|:---|:---| 
|**Name** | **Description** |
|__Total CPU Time__|Total Enlighten CPU time across all threads.|
|__Probe Update Time__| Time spent updating Light Probes.|
|__Setup Time__|Time spent in the Setup stage.|
|__Environment Time__ |Time spent processing Environment lighting.|
|__Input Lighting Time__|Time spent processing input lighting.|
|__Systems Time__|Time spent updating Systems.|
|__Solve Tasks Time__|Time spent running radiosity solver tasks.|
|__Dynamic Objects Time__|Time spent updating Dynamic GameObjects.|
|__Time Between Updates__|Time between Global Illumination updates.|
|__Other Commands Time__|Time spent processing other commands.|
|__Blocked Command Write Time__|Time spent in blocked state.|
|__Blocked Buffer Writes__|Number of blocked writes.|
|__Total Light Probes__|Total number of Light Probes in the Scene.|
|__Solved Light Probes__|Number of solved Light Probes since the last update.|
|__Probe Sets__|Number of Light Probe sets in the Scene.|
|__Systems__|Number of Enlighten Systems in the Scene.|

---
* <span class="page-edit">2017-08-30  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">Unity 2017.2 中的新功能</span>









