<p align="center"><img alt="Pyroscope" src="https://github.com/grafana/pyroscope/assets/662636/c1fc4055-b33d-4e69-a450-9e7a7b2317bb" width="100%"/></p>


[![ci](https://github.com/grafana/pyroscope/actions/workflows/test.yml/badge.svg)](https://github.com/grafana/pyroscope/actions/workflows/test.yml)
[![JS Tests Status](https://github.com/grafana/pyroscope/workflows/JS%20Tests/badge.svg)](https://github.com/grafana/pyroscope/actions?query=workflow%3AJS%20Tests)
[![Go Report](https://goreportcard.com/badge/github.com/grafana/pyroscope)](https://goreportcard.com/report/github.com/grafana/pyroscope)
[![License: AGPLv3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](LICENSE)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fgrafana%2Fpyroscope.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fgrafana%2Fpyroscope?ref=badge_shield)
[![Latest release](https://img.shields.io/github/release/grafana/pyroscope.svg)](https://github.com/grafana/pyroscope/releases)
[![DockerHub](https://img.shields.io/docker/pulls/grafana/pyroscope.svg)](https://hub.docker.com/r/grafana/pyroscope)
[![GoDoc](https://godoc.org/github.com/grafana/pyroscope?status.svg)](https://godoc.org/github.com/grafana/pyroscope)

## 🎉 **公告：全新的 Explore Profiles UI 现已发布！**

我们很高兴地宣布推出 **Explore Profiles UI**，这是一种全新的探索和分析性能分析数据的方式——现已作为 Grafana Explore Apps 套件的一部分提供！这个新应用为您带来了**无需查询**、**直观**的性能分析数据可视化体验，无需编写复杂的查询即可简化整个流程。

https://github.com/user-attachments/assets/4db19ec7-86f3-4701-8f5f-9b7ffcebd49c

## 什么是 Grafana Pyroscope？

Grafana Pyroscope 是一个持续性能分析平台，旨在从您的应用程序中揭示性能洞察，帮助您优化资源使用，如 CPU、内存和 I/O 操作。使用 Pyroscope，您可以**主动**和**被动**地解决系统中的性能瓶颈。

典型用例包括：

- **主动：** 减少资源消耗、提高应用程序性能或防止延迟问题。
- **被动：** 快速解决事件，提供行级详细信息，调试活跃的 CPU、内存或 I/O 瓶颈。

Pyroscope 提供强大的工具，让您全面了解应用程序的行为，同时允许您深入特定服务以进行更有针对性的根本原因分析。

## Pyroscope 如何工作？

![deployment_diagram](https://grafana.com/media/docs/pyroscope/pyroscope_client_server_diagram_09_18_2024.png)

Pyroscope 由三个主要组件组成：
- **Pyroscope 服务器：** 存储和处理性能分析数据的服务器组件。
- **Pyroscope SDK（推送）或 Grafana Alloy（拉取）：** Pyroscope 的客户端部分，从您的应用程序收集性能分析数据并发送到服务器。
- **Explore Profiles UI：** 一个无需查询、直观的 UI，用于可视化和分析性能分析数据。

---

## [Pyroscope 实时演示](https://play.grafana.org/a/grafana-pyroscope-app/)

[![Pyroscope GIF Demo](https://github.com/user-attachments/assets/2faeb985-f2b6-4311-ad29-e318e850c025)](https://play.grafana.org/a/grafana-pyroscope-app/)


---

## **快速开始：在本地运行 Pyroscope 服务器**

### Homebrew
```sh
brew install pyroscope-io/brew/pyroscope
brew services start pyroscope
```

### Docker
```sh
docker run -it -p 4040:4040 grafana/pyroscope
```

有关如何配置 Pyroscope 服务器的更多文档，请参阅[我们的服务器文档](https://grafana.com/docs/pyroscope/latest/configure-server/)。

## **快速开始：在 Grafana 中运行 Explore Profiles UI**

<img width="1728" alt="image" src="https://github.com/user-attachments/assets/67691443-6450-45b9-8064-f41056c88ade">

### Grafana Cloud
应用 UI 和服务器都会自动安装并运行——只需开始发送数据即可！

### Grafana OSS
您可以通过从 [Grafana 插件目录](https://grafana.com/grafana/plugins/grafana-pyroscope-app/) 安装插件，在 Grafana 中运行 Explore Profiles UI。

更多信息，请查看 [Explore Profiles README](https://github.com/grafana/explore-profiles)

## 文档

有关如何将 Pyroscope 与其他编程语言一起使用、在 Linux 上安装或在生产环境中使用的更多信息，请查看我们的文档：

* **[📘 Pyroscope 与 Grafana 集成使用指南（中文）](GRAFANA_INTEGRATION_GUIDE_CN.md)** - 详细说明如何将 Pyroscope 与 Grafana 结合使用
* [入门指南](https://grafana.com/docs/pyroscope/latest/get-started/)
* [部署指南](https://grafana.com/docs/pyroscope/latest/deploy-kubernetes/)
* [Pyroscope 架构](https://grafana.com/docs/pyroscope/latest/reference-pyroscope-architecture/)

## 通过 Pyroscope 代理（特定语言）向服务器发送数据

有关如何在代码中添加 Pyroscope 代理的更多文档，请参阅我们网站上的[代理文档](https://grafana.com/docs/pyroscope/latest/configure-client/)，或查看下面的特定语言示例和文档：
<table>
   <tr>
      <td align="center"><a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/go_push/"><img src="https://user-images.githubusercontent.com/23323466/178160549-2d69a325-56ec-4e19-bca7-d460d400b163.png" width="100px;" alt=""/><br />
        <b>Golang</b></a><br />
          <a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/go_push/" title="Documentation">文档</a><br />
          <a href="https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/golang-push" title="golang-examples">示例</a>
      </td>
      <td align="center"><a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/java/"><img src="https://user-images.githubusercontent.com/23323466/178160550-2b5a623a-0f4c-4911-923f-2c825784d45d.png" width="100px;" alt=""/><br />
        <b>Java</b></a><br />
          <a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/java/" title="Documentation">文档</a><br />
          <a href="https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/java/rideshare" title="java-examples">示例</a>
      </td>
      <td align="center"><a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/python/"><img src="https://user-images.githubusercontent.com/23323466/178160553-c78b8c15-99b4-43f3-a2a0-252b6c4862b1.png" width="100px;" alt=""/><br />
        <b>Python</b></a><br />
          <a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/python/" title="Documentation">文档</a><br />
          <a href="https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/python" title="python-examples">示例</a>
      </td>
      <td align="center"><a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/ruby/"><img src="https://user-images.githubusercontent.com/23323466/178160554-b0be2bc5-8574-4881-ac4c-7977c0b2c195.png" width="100px;" alt=""/><br />
        <b>Ruby</b></a><br />
          <a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/ruby/" title="Documentation">文档</a><br />
          <a href="https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/ruby" title="ruby-examples">示例</a>
      </td>
   </tr>
   <tr>
      <td align="center"><a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/nodejs/"><img src="https://user-images.githubusercontent.com/23323466/178160551-a79ee6ff-a5d6-419e-89e6-39047cb08126.png" width="100px;" alt=""/><br />
        <b>Node.js</b></a><br />
          <a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/nodejs/" title="Documentation">文档</a><br />
          <a href="https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/nodejs/express" title="examples">示例</a>
      </td>
      <td align="center"><a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/dotnet/"><img src="https://user-images.githubusercontent.com/23323466/178160544-d2e189c6-a521-482c-a7dc-5375c1985e24.png" width="100px;" alt=""/><br />
        <b>.NET</b></a><br />
          <a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/dotnet/" title="Documentation">文档</a><br />
          <a href="https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/dotnet" title="examples">示例</a>
      </td>
      <td align="center"><a href="https://grafana.com/docs/pyroscope/latest/configure-client/grafana-alloy/ebpf/"><img src="https://user-images.githubusercontent.com/23323466/178160548-e974c080-808d-4c5d-be9b-c983a319b037.png" width="100px;" alt=""/><br />
        <b>eBPF</b></a><br />
          <a href="https://grafana.com/docs/pyroscope/latest/configure-client/grafana-alloy/ebpf/" title="Documentation">文档</a><br />
          <a href="https://github.com/grafana/pyroscope/tree/main/examples/grafana-alloy-auto-instrumentation/ebpf" title="examples">示例</a>
      </td>
      <td align="center"><a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/rust/"><img src="https://user-images.githubusercontent.com/23323466/178160555-fb6aeee7-5d31-4bcb-9e3e-41e9f2f7d5b4.png" width="100px;" alt=""/><br />
        <b>Rust</b></a><br />
          <a href="https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/rust/" title="Documentation">文档</a><br />
          <a href="https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/rust/rideshare" title="examples">示例</a>
      </td>
   </tr>
</table>

## [支持的语言][supported languages]

我们的文档包含最新的[支持的语言][supported languages]列表，以及每种语言支持的[性能分析类型][profile-types-languages]的概述。

如果您想看到其他集成，请在[我们的 issues](https://github.com/grafana/pyroscope/issues?q=is%3Aissue+is%3Aopen+label%3Anew-profilers) 或[我们的 slack](https://slack.grafana.com) 中告诉我们。

[supported languages]: https://grafana.com/docs/pyroscope/latest/configure-client/
[profile-types-languages]: https://grafana.com/docs/pyroscope/latest/configure-client/profile-types/

## 致谢

Pyroscope 的实现得益于许多人的出色工作，包括但不限于：

* Brendan Gregg — 火焰图的发明者
* Julia Evans — rbspy 的创建者 — Ruby 的采样性能分析器
* Vladimir Agafonkin — flamebearer 的创建者 — 快速火焰图渲染器
* Ben Frederickson — py-spy 的创建者 — Python 的采样性能分析器
* Adam Saponara — phpspy 的创建者 — PHP 的采样性能分析器
* Alexei Starovoitov、Daniel Borkmann 以及许多其他在 Linux 内核中实现基于 BPF 的性能分析的人
* Jamie Wong — speedscope 的创建者 — 交互式火焰图可视化工具

## 贡献

要开始贡献，请查看我们的[贡献指南](docs/internal/contributing/README.md)


### 感谢 Pyroscope 的贡献者！


