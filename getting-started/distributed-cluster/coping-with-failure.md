
应对失败
========


* [空集群](an-empty-cluster.md)
* [集群健康](cluster-health.md)
* [添加索引](add-an-index.md)
* [故障转移](add-failover.md)
* [水平扩展](scale-horizontally.md)
* [应对失败](coping-with-failure.md)

我们已经说过当有节点损失时Elasticsearch可以自动复制，让我们开始尝试。如果我们杀掉第一个节点，我们的集群看起来像图6：“杀掉一个节点后的集群”。

图6. 杀掉一个节点后的集群
![杀掉一个节点后的集群](elas_0206.png)


我们杀掉的节点是主节点。一个集群要功能运转正常，必须有一个主节点，所以第意见事情就是这些节点选举出一个新的主节点：Node 2。

杀掉Nod 1时主分片1和2丢失了，失去主分片后索引是不能正常工作的。如果我们在这个时候检查集群的健康，我们将看到`red`状态：不是所有的主分片都正常！

幸运的是，失去主分片后在其他节点还有它们的完整拷贝，因此新主节点要做的第一件事就是将Node 2和Node 3节点上这两个复制分片变成主分片，将集群健康状态变成`yellow`。这个推动过程是瞬间的，就像按下一个开关。

那为什么集群的健康是`yellow`而不是`green`状态呢？我们已经有三个主分片，但我们规定了每个主分片要对应2个复制分片，而现在只分配了一个。这阻止我们达到`green`的标准，不过我们不用太担心这个：即使我们把Node 2也杀掉，我们的程序可以继续运行，不会丢失数据，因为Node 3包含了所有分片的拷贝。

如果我们重新启动Node 1，集群将能重新分配失去的复制分片，最后的状态类似于图5：“复制分片数量增长到2”。如果节点1仍然有旧分片的数据，将会尝试重新利用它们，与此同时将主分片里边已经改变的数据拷贝过去。

到现在为止，我们对分片允许Elasticsearch进行水平扩展，以及如果确保数据安全有了合理的想法。后面我们将具体研究分片的生命周期。


--------

[« 水平扩展](scale-horizontally.md)     [数据 »](../data-in-data-out/README.MD)
