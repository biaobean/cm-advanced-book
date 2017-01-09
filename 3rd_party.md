# Cloudera托管服务开发

Cloudera Manager是一个功能强大的工具，用于管理CDH服务和运行它们的集群。通过使用Cloudera Manager扩展机制，可以使用Cloudera Manager来一起同管理CDH服务以及非Cloudera提供的服务，并且为这样的管理服务部署插件和扩展。CM托管服务的开发请参考[CM_ext项目](https://github.com/cloudera/cm_ext)，文档请参见CM扩展功能[Wiki主页](https://github.com/cloudera/cm_ext/wiki)。

总体上讲，Cloudera Manager本身涉及管理群集上运行的各种服务的生命周期。这可以被分解为两个主要领域：

1. 管理该服务所需程序/数据文件的部署
2. 管理和监控这些服务的配置和运行

因此，CM托管服务的开发内容包括两个方面：

1. 将需要管理的二进制服务包按照Cloudera定义的Parcel包格式封装，用于部署应用程序包。欲了解Parcel包的信息，请参见https://blog.cloudera.com/blog/2013/05/faq-understanding-the-parcel-binary-distribution-format/。
2. 将服务管理、安装、监控等操作按照CSD的格式编写，用于说明如何进行服务管理，并将其使用CM来管理生命周期以及监控。请参见https://github.com/cloudera/cm_ext/wiki/CSD-Overview/。

## 参考资源

1. CM扩展功能Wiki主页： [https://github.com/cloudera/cm\_ext/wiki](https://github.com/cloudera/cm_ext/wiki)
2. 示例： [https://github.com/ggear/cloudera-parcel](https://github.com/ggear/cloudera-parcel)