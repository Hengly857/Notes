# mongodb优势
MongoDB 作为非关系型数据库（NoSQL）的一种，具有以下优势：
灵活性：MongoDB 使用 BSON（二进制 JSON）格式存储数据，这意味着文档可以有不同结构。这种灵活性使得在项目需求变化时更容易适应，不需要进行复杂的数据库迁移。
水平扩展性：MongoDB 支持分片（sharding），可以将数据分布到多个服务器上，从而支持海量的数据存储和高吞吐量的应用程序。
高性能：对于某些类型的操作，如插入、查询等，MongoDB 提供了非常高的性能，特别是当索引被正确使用时。
丰富的查询语言：MongoDB 提供了一套强大的查询语言，能够执行复杂查询，包括排序、聚合、文本搜索等。
高可用性和容错性：通过复制集（replica sets），MongoDB 能够提供自动故障转移和数据冗余，提高了系统的可靠性和稳定性。
地理空间功能：MongoDB 内置了对地理空间查询的支持，这对于需要处理地理位置信息的应用非常有用。

# MongoDB 的服务层
在使用 MongoDB 的服务层中，不会使用 SQL 进行检索。而是使用 MongoDB 自己的查询语言，该语言是基于 JSON/BSON 格式的，并且可以通过多种编程语言的驱动来构建查询。
例如，在 Node.js 中可以使用 Mongoose，这是一种设计用来与 MongoDB 交互的对象建模工具；而在 Java 中可以使用 Mongo Java Driver 来构建查询命令。
以下是用 MongoDB 查询语言进行检索的一些例子：
查找集合中的所有文档: db.collection.find()
查找满足特定条件的文档: db.collection.find({ "key": "value" })
使用聚合管道进行复杂查询: db.collection.aggregate([ { $match: { "key": "value" } }, ... ])
每个操作都可以通过相应的 MongoDB 驱动程序以编程方式实现。