## Impala

必须现在Impala中启用动态资源管理，才会在集群-&gt;Dynamic Resource Pool Configuration中看到Impala的动态资源配置界面。

首先在Impala配置界面中的“类别”栏中选取“Admission Control”，然后开启“启用 Impala Admission Control”和“启用动态资源池”。通常这两个功能需要同时禁用或同时启用。

![](image/resource_pool/impala_ac_conf.png)

然后在集群-&gt;Dynamic Resource Pool Configuration中可以看到“Impala Admission Control“的Tab：

![](image/resource_pool/yarn_impala_tab.png)

