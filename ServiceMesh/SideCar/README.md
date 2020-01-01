# Sidecar（挎斗）

其中，对于服务网格创建的通信层应该位于架构的位置，Aspen Mesh 的 Andrew Jenkins 就提出了三种可能的选择，即：

- 作为一个 Lib 库，让每个微服务可导入。
- 部署在节点的代理服务中，为所在节点的全部容器提供服务。
- 与应用容器相并行，运作在一个独立的 sidecar 容器内。

其中，基于 sidecar 模式是目前最流行的服务网格模式，以至于在某种程度上讲，它已经成等同于服务网格的模式。

![Sidercar 架构](https://s2.ax1x.com/2019/11/02/KLEbgf.png)

服务网格中的每个应用微服务容器都有一个对应匹配的代理容器。服务到服务的通信所需的所有逻辑请求都从微服务中抽象出来，放到了 sidecar 中处理。通过将所有网络和通信代码放到一个单独的容器中，使其成为基础设施的一部分，并使开发人员不必将如上的内容视为应用程序的一部分而投入精力处理。从本质上说，抽象后剩下的也是一个微服务，但它可以非常专注于其业务逻辑。在微服务所运行的复杂环境中，它不需要知道如何与其他的任何微服务实现通信的问题，它只需要知道如何与 sidecar 通信，然后由 sidecar 负责余下的工作。

# 为什么需要 Sidecar？

在构建任何软件架构时，我们都会不可避免地引入通过在网络上发出请求来实现彼此通信的服务。例如，考虑任何与数据库通信以存储或检索数据的应用程序，或者考虑一个更复杂的面向微服务的应用程序，它们为了执行操作都要向不同的服务发出许多请求：

![网络连接方式](https://s2.ax1x.com/2019/10/27/KynxII.jpg)

每当我们的服务通过网络请求互连时，我们都将最终用户的体验置于风险之中。我们都知道，不同服务之间的连接可能很慢，而且无法预测。它可能不安全，难以跟踪，并会引发许多其他的问题（例如路由、版本控制、金丝雀部署）。

针对这种情况，开发人员通常采取以下一种措施：

- 编写更多代码：开发人员构建一个智能客户端，每个服务都必须以库的形式使用该客户端。通常，这种方法会带来以下几个新问题：导致了更多技术债务；通常特定于具体的语言，妨碍了创新；库有多种实现，长远来看会导致碎片化。

- 挎斗（Sidecar）代理：服务将所有连接性和可观察性关注点委托给进程外运行时，后者位于每个请求的执行路径上。它将代理所有发出的连接并接受所有传入的连接。使用这种方法，开发人员就不需要关心连接性，而只需要关注业务价值交付。

## 数据平面与控制平面

它之所以被称为挎斗代理，是因为它是同一主机上与我们的服务进程并行运行的另一个进程，就像摩托车跨斗一样。服务的每个运行实例都将有一个挎斗代理实例，由于所有传入和传出的请求——以及它们的数据——总是通过挎斗代理，所以它也被称为数据平面（DP）。

挎斗代理模型需要一个控制平面，使团队可以配置数据平面的行为并跟踪其服务的状态。采用挎斗代理模型的团队要么从头开始构建一个控制平面，要么使用市场上现有的通用控制平面，比如 Kuma。

与数据平面（DP）不同，控制平面（CP）并不位于服务交互请求的执行路径上，它用于配置数据平面并从中检索数据（如可观察性信息）。

![数据平面与控制平面](https://s2.ax1x.com/2019/10/27/Kyyvin.jpg)

服务网格：一种由数据平面（DP：和服务平行部署的挎斗代理）和控制 DP 的控制平面（CP）组成的架构。通常，服务网格出现在 Kubernetes 上下文中，但是任何人都可以在任何平台上构建服务网格（包括 VM 和裸机）。