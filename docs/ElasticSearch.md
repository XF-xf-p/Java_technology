## 1.ElasticSearch

ElasticSearch是一个分布式、基于RESTful风格的数据搜索和分析引擎，由Elastic公司开发并基于Apache许可条款发布源码。ElasticSearch的底层全文检索基于Lucene实现，其灵活的数据存取和分析方式、良好的性能和稳定性使其在大数据存储和分析领域被广泛使用。

## 2.Lucene简介

ElasticSearch的底层存储基于Lucene实现，Lucene是Apache软件基金的一个开源子项目，是一套全文检索引擎架构，提供了完整的文本分析引擎、数据查询引擎和数据索引引擎。Lucene的目的是为软件开发人员提供一个简单易用的工具包，以便在目标系统中实现全文检索的功能，或者以Lecene为基础建立一套完整的全文检索引擎。

## 3.倒排排索

在实际应用中，我们常常需要根据属性的值来查找记录，这时就需要使用倒排索引( Inverted Index）。倒排索引表中的每一项都包括一个属性值和具有该属性值对应记录的地址。由于不是按照记录来确定属性值的，而是由属性值来确定记录的位置的，因此被称为倒排索引。例如，当我们在百度搜索栏中输入关键词时，百度会按照输入的关键词在所有文档内容（比如网页内容）中搜索与关键词相关的记录（比如网站），并将内容相关的记录的地址（比如网站的地址）返回用户，然后用户便可按该记录的地址进一步查看记录的详细信息。

搜索引擎的关键是建立倒排索引，倒排索引一般表示一个关键词，以及它的频度（出现的次数）和位置（出现在哪一篇文章或网页中，以及相关的日期、作者等信息）。

带有倒排索引的文件被称为倒排索引文件（Inverted File）。 倒排索引的索引对象是文档或者文档集合中的单词，倒排索引文件被用来存储这些单词在一个文档或者一组文档中的位置。

在搜索引擎中，每个文档都有一个对应的文档 ID，文档内容被表示为一系列关键词的集合。例如，文档 1 经过分词，提取了 20 个关键词，每个关键词都会记录它在文档中出现的次数和出现位置。

那么，倒排索引就是**关键词到文档** ID 的映射，每个关键词都对应着一系列的文件，这些文件中都出现了关键词。

举个栗子。

有以下文档：

| DocId | Doc                                            |
| ----- | ---------------------------------------------- |
| 1     | 谷歌地图之父跳槽 Facebook                      |
| 2     | 谷歌地图之父加盟 Facebook                      |
| 3     | 谷歌地图创始人拉斯离开谷歌加盟 Facebook        |
| 4     | 谷歌地图之父跳槽 Facebook 与 Wave 项目取消有关 |
| 5     | 谷歌地图之父拉斯加盟社交网站 Facebook          |

对文档进行分词之后，得到以下**倒排索引**。

| WordId | Word     | DocIds    |
| ------ | -------- | --------- |
| 1      | 谷歌     | 1,2,3,4,5 |
| 2      | 地图     | 1,2,3,4,5 |
| 3      | 之父     | 1,2,4,5   |
| 4      | 跳槽     | 1,4       |
| 5      | Facebook | 1,2,3,4,5 |
| 6      | 加盟     | 2,3,5     |
| 7      | 创始人   | 3         |
| 8      | 拉斯     | 3,5       |
| 9      | 离开     | 3         |
| 10     | 与       | 4         |
| ..     | ..       | ..        |

另外，实用的倒排索引还可以记录更多的信息，比如文档频率信息，表示在文档集合中有多少个文档包含某个单词。

那么，有了倒排索引，搜索引擎可以很方便地响应用户的查询。比如用户输入查询 `Facebook`，搜索系统查找倒排索引，从中读出包含这个单词的文档，这些文档就是提供给用户的搜索结果。

要注意倒排索引的两个重要细节：

- 倒排索引中的所有词项对应一个或多个文档；
- 倒排索引中的词项**根据字典顺序升序排列**

## 4.Lucene的架构

Lucene是一个高并发、高吞吐、可扩展的全文检索库。它基于Java实现，使用方便。Lucene内部的数据结构叫作文档（Document），当应用层的数据（例如，FieSystem、Web Data、DataBase等）进入Lucene时，首先会进行索引文档（Index Document ）操作，按照索引规则创建倒排索引；在应用程序查询数据的时候，直接查询提建好的倒排索引，因此其效率十分高。

简单来说，lucene 就是一个 jar 包，里面包含了封装好的各种建立倒排索引的算法代码。我们用 Java 开发的时候，引入 lucene jar，然后基于 lucene 的 api 去开发就可以了。

通过 lucene，我们可以将已有的数据建立索引，lucene 会在本地磁盘上面，给我们组织索引的数据结构。

## 5.Lucene的全文检索流程

Lucene的全文检索流程包括索引创建和索引查询两个过程。其中，索引创建分为获取文档、构建文档对象、文档分词和创建索引4个步骤。查询索引分为调用查询接口、创建查询、执行查询和结果返回4个步骤。

1 ）创建索引。

创建索引指对用户要搜索的文档内容执行创建索引操作，并将索引结果存储在倒排索引库中。具体步骤如下。

( 1 ）获取文档：获取文档的过程即数据采集的过程。Lucene中的文档指要索引和搜索的原始内容。文档内容可以是互联网上的网页、数据库中的数据、磁盘上的日志文件等。

( 2 ）构建文档对象：当获取文档内容后，需要根据文档内容构建文档（Document）对象，每个文档对象都包含一个唯一的文档id和多个Field，每个Field中都存储着不同的文档内容。例如，将磁盘上一个包含一篇文章的TXT文件当成一个Document，则Document中包含多个Field。每个Field都包含不同的内容，比如file_name （文件名称）、file_path （文件路径）、file_size（文件大小）、file_content （文件内容）。

Field的属性配置如下。

Tokenized（是否分词）：是否对Field的内容进行分词处理。当我们要对Field内容进行查询时，需要开启分析功能。

Indexed（是否索引）：是否将Field分词后的词或整个Field值进行索引，只有经过索引的Field才能被搜索。

Stored（是否存储）：是否将Field值存储在文档中，只有存储在文档中的Field值才可以从文档中被获取。

( 3 ）分析文档：分析文档的过程是将原始内容创建为包含Field的文档（Document）并对Field的内容进行分析的过程。分析文档的过程需要对原始文档执行提取单词、大小写转换、去除标点符号、去除停用词等操作，然后生成最终的语汇单元。如下为一个分析文档的过程。

- 原文档内容：Lucene is a Java full-text search engine
- 分析后得到的语汇单元：lucene、java、full、search、engine。

语汇单元中的每个单词都被叫作一个Term，不同的Field拆分出来的相同单词是不同的Term。Term中包含两部分：一部分是文档的Field名称，另一部分是单词的内容。

( 4 ）创建索引：创建索引指对所有文档分析得出的Term都进行索引并记录该Term在每个Document中出现的次数的过程，索引的目的是搜索，最终要通过搜索被索引的语汇单元查找文档信息。

2 ）查询索引。

查询索引即根据用户输入的关键字，从索引（Index ）中进行搜索的过程。查询索引的具体过程为：根据关键字搜索索引，根据索引找到对应的文档，从而找到要搜索的内容。

( 1 ）用户查询接口：全文检索系统提供的用户搜索界面，实现用户搜索关键字或关键词的提交，以及搜索完成后搜索结果的展示。

( 2 ）创建查询对象：用户在输入关键字执行搜索之前，需要先构建一个查询对象。查询对象中可以指定要搜索的文档Field、关键字等。查询对象会生成具体的查询语法（例如“fileName:Lucene"表示要搜索Field的内容为“Lucene”的文档）。

( 3）执行查询：根据查询语法在倒排索引词典表中分别找出对应搜索词的索引，从而找到索引对应的文档链表。搜索过程为在索引中查找Field为fileName且关键字为Lecene的Term，然后根据Term找到对应的文档id列表。

( 4 ）返回查询结果：将查询的文档id列表返回到用户查询接口。

## 6.Lucene的使用

ElasticSearch底层索引基于Lucene全文检索实现。

( 1 ）新建Lecene的Mean项目，并添加以下Lucene依赖包。

( 2 ）定义Article的数据结构。

新建Article类用于表示一篇文章的数据结构，其中包括文章id、文章标题（title ）、文章作者（author）、文章内容（content ）、文章位置（URL）。这里定义的Article的数据结构表示Lucene接收的原始数据格式。

( 3 ）定义Index的创建方法.

创建Index前，先将接收的原始数据结构转换为Lucene对应的Document格式。

Document doc ＝口ewDocument(); 

当将元数据（Artcle数据）转换为文档数据后，便可以按照如下代码建一个索引其核步骤包括创建原始数据、创建分词器、创建文档和写入索引

( 4 ）定义Index的查询方法。

定义LeceneTest类并在LeceneTest中按照如下代码定义Index的查询方法。其核心步骤包括定义分词器、定义索引器、定义查询条件、解析查询条件、执行查询、查询结果获取和返回。

( 5 ）索引创建和查询。

上述（1 )~( 4）的代码首先调用createlndex创建一个索引。在执行完成后，可以看到在索引目录下生成了以下索引数据。其中，.cfe和.cfs的文件表示复合文件(Compound File）; .si的文件表示段信息（Segments Info); segments_1表示第一个段文件（Segments File); write.lock表示锁文件，用以阻止多个indexWriter向同一个文件写数据。

## 7.ElasticSearch的特点

ElasticSearch为大数据提供了稳定、可靠、快速的数据存储和查询服务，是大数据开发中最常用的数据库组件之一。其主要特点如下。

( 1 ）高容量：ElasticSearch集群支持PB级数据的存储和查询。

( 2 ）高吞吐：ElasticSearch支持对海量数据近实时的数据处理。

( 3 ）高可用：ElasticSearch基于副本机制支持部分服务宕机后仍可正常运行和使用。

( 4 ）支持多维度数据分析和处理：除了支持全文检索，ElasticSearch还支持基于单字段精确查询和多字段联合查询等复杂的数据查询操作.

( 5) API简单易用：ElasticSearch API简单易用，除了支持REST API，还支持Java、Python等多种客户端形式，且查询方式简单灵活。

( 6 )支持插件机制：ElasticSearch支持插件式开发，基于ElasticSearch 可以开发自己的分词插件、同步插件、Hadoop插件、可视化插件等。

## 8.ElasticSearch的应用场景

ElasticSearch的应用场景广泛，不但可以用于全文检索、分布式存储，还可用于系统运维、日志监控和BI系统。以下为常见的ElasticSearch应用场景。

( 1 ）全文检索：ElasticSearch底层基于Lucene实现，十分适合类似百度百科、维基百科等全文检索的应用场景。

( 2 ）分布式数据库：ElasticSearch可作为分布式数据库，为大数据云计算提供数据存储和查询服务，广泛应用于淘宝、京东等电商平台的商品管理和检索服务。

( 3 ）日志分析：通过Logstash等日志采集组件，ElasticSearch可实现复杂的日志数据存储分析和查询，最常用的组合是ELK( ElasticSearch+Logstash+Kibana）技术组合。

( 4 ）运维监控：运维平台可以基于ElasticSearch实现大规模服务的监控和管理。

( 5 ) BI系统：ElasticSearch广泛应用于BI( Business Intelligence，商业智能）系统，例如按照区域统计用户的操作习惯等。

## 9.ElasticSearch的数据模型

ElasticSearch的数据模型由Index（索引）、Type（类型）和Document （文档）组成。索引是一组具有共同特征的文档集合。每个索引都包含多个类型，每个类型都包含多个文档，每个文档都包含多个Field。

### Index

Index （索引）是一组具有共同特征的文档集合，每个Index都有自己的Mapping，Mapping用于定义所包含的文档的字段名和字段类型。同一个索引中的数据在物理上被分散在多个Shard上以实现负载均衡。

### Type 

Type （类型）用于在索引中提供一个逻辑分区。它用于表示有类似结构的文档。一个索引可以有多个类型。例如，将图书馆的所有书看成一个索引，则该索引下包含多种类型（如计算机、天文、地理、医学、数学等）的图书，这些类型有相似的数据结构，比如都含有书名、出版社、作者等。

注意：Type（类型）的概念只在ElasticSearch 6.x及之前的版本存在，在ElasticSearch 7.x之后已经被移除。

### Document 

ElasticSearch是面向Document（文档）的，文档ElasticSearch数据存储和索引的最小单位。文档以序列化JSON格式保存在ElasticSearch种。每个文档都有一个文档类型和文档id，id是文档的唯一标识。一个简单的文档结构如下。

```
{
	"_index":"library",
	"_type":"class_computer",
	"_id":"1",
	"_version":1,
	"found":true,
	"_source":{
		"book_name":"offer"
	}
}
```

上述代码描述了一个Index为library（图书馆）、Type为class_computer （计算机类图书）、id为1的文档，文档的元数据中包含一个book_name，其对应的值为“Offer”。

### Field

Field是文档内的一个基本单位，以键值对的形式存在（例如“book_name”：“Offer”）。

### Mapping

Mapping （映射）用于设置文档的每个Field及其对应的数据类型（例如，字符串、整数、浮点数、双精度数、日期等）。在创建索引的过程中，ElasticSearch会自动创建一个针对Field的映射，根据特定的数据类型，可以很容易地查询或修改这些映射。

### Shard

ElasticSearch将一个大的Index数据拆分为多个小Shard（分片），分布在不同物理服务器上，以实现数据的分布式存储和查询。分片分为主分片（Primary Shard ）与副本分片（Replica Shard）。主分片和副本分片通常分布在不同节点上，用于故障转移和负载均衡。在节点故障或新节点加入时，分片可以从一个节点移动到另一个节点。一个索引可以有多个主分片和副本分片。

## 10.ElasticSearch分布式架构

ElasticSearch基于分布式的架构能够支撑PB级数据的搜索和分析。ElasticSearch分布式架构的核心内容包括集群节点角色、集群选举原理、集群状态、数据路由规则、数据分片和副本策略等

### 集群节点角色

ElasticSearch集群节点角色包括MasterNode（主节点）、DataNode（数据节点）、IngestNode（提取节点）、CoordinatingNode（协调节点）和TribeNode（部落节点）。

( 1 ) MasterNode （主节点）：MasterNode主要负责集群节点状态的维护、索引的创建删除、数据的Rebalance、分片的分配等工作。MasterNode不负责具体数据的索引和检索，因此其负载较低，服务比较稳定。当MasterNode宕机时，ElasticSearch集群会自动从其他MasterNode中选举出一个Leader继续为集群提供服务。为了防止在选举过程中出现脑裂现象，常常需要设置discovery.zen.minimum_ master_nodes=N/2+1，其中N为集群中MasterNode的个数。建议集群中MasterNode的个数为奇数，如3个或者5个.节点只包含MasterNode角色的配如下

```
node.master: true ＃设置节点角色为MasterNode
node.data: false ＃设置节点角色为非DataNode
node.ingest: false ＃设置节点角色为非IngestNode
search.remote.connect:false ＃设置节点角色为非查询角色
```

在一般生产环境中，为了保障MasterNode的稳定运行，不建议在MasterNode上配置数据节点。

( 2 ) DataNode （数据节点）：DataNode是集群的数据节点，主要负责集群中数据的索引创建和检索，具体操作包括数据的索引、搜索、聚合等。DataNode属于I/O、内存和CPU密集型操作，需要的计算资源较大，如果资源允许，则建议使用SSD以加快数据读写的效率。

( 3 ) IngestNode （提取节点）：IngestNode是执行数据预处理的管道，它在索引之前预处理文档。通过拦截文档的Bulk和Index请求，然后加以转换，最终将文档传回Bulk和Index API，用户可以定义一个管道，指定一系列预处理器。如果集群有复杂的数据预处理逻辑，则该节点属于高负载节点，建议使有专用服务器。

( 4) CoordinatingNode（协调节点）：CoordinatingNode用于接收客户端请求，并将请求转发到各个DataNode上。各个DataNode在收到请求后，在本地执行请求操作，并将请求结果反馈给CoordinatingNode，CoordinatingNode在收到所有DataNode的反馈后，进行结果合并，然后将结果返回客户端。

( 5 ) TribeNode（部落节点）：允许TribeNode在多个集群之间充当联合客户端，用于实现跨集群访问。在5.4.0版本以后，TribeNode已经被废弃，并不建议使用，其替代方案为cross-cluster Search。

### 集群选举原理

ElasticSearch在集群启动、集群重启和Master失效后会触发集群选主操作。ElasticSearch集群选主操作采用Bully算法实现。

( 1 ) Bully算法。

Bully算法是Leader选举的基本算法之一，其优点是易于实现。该算法假定所有节点都有一个唯一的id，使用该id对节点进行排序，每次都会选出存活的进程中id最大的节点作为候选者。需要注意的是，ElasticSearch在选举算法的具体实现过程中，其选举实现ElectMasterService将该算法的实现进行了修改，选用最小id作为候选者。

( 2 ）集群脑裂问题。

Bully算法实现简单，且选举效率高，但是在集群网络不稳定的情况下容易出现集群脑裂现象。脑裂现象指在分布式集群中各节点之间由于网络分区而不能正常通信，使得原本为一个整体的集群为裂为两个或多个集群，从而导致系统混乱、服务异常、数据不一致的现象。为防止脑裂现象的发生，ElasticSearch通过discovery.zen.minimum_master_nodes来控制选主的条件。其配置如下。

discovery.zen.minimum_master_nodes= (master_eligible_nodes)/2+1 

在上述配置中，（master_eligible_nodes) /2+1 表示大于半数节点个数，ElasticSearch基于该配置在集群选主的时候会做如下判断。

1.触发选主：在进入选举临时Master之前，参选的节点数需要达到法定数量。

2.决定Master：在选出临时Master之后，得票数需要达到法定数量，才确认选主成功。

3.Gateway选举元信息：向有Master资格的节点发起请求，获取元数据，获取的响应数量必须达到法定数量，也就是参与元信息选举的节点数。

4.Master发布集群状态：成功向节点发布集群状态信息的数量要达到法定数量。

5.NodesFaultDetecton事件中是否触发rejoin：当发现有节点连不上时，会执行removeNode。接着审视此时的法定数量是否大于discovery.zen.minimum_master_nodes配置，如果不大于，则主动放弃Master身份执行rejoin以避免脑裂。

(  3 ）选举原则。

ElasticSearch的选举原则如下。

1.每个节点计算最小的已知节点id，并向该节点投票。

2.如果一个节点获取足够多票数，并且该节点也为自己投票，那么它将成为Leader角色，开始发布集群状态。

3.ElasticSearch中的所有节点都会参与选举和投票，但只有拥有Master角色的节点投票才有效。

### 集群状态

ElasticSearch集群的状态分为Green、Yellow和Red3种。 

( 1 ) Green ：所有主分片和副本分片都运行正常。

( 2) Yellow ：所有主分片都运行正常，但至少还有一个副本分片运行异常。在这种状态下，集群仍然可以对外提供服务，并且可以保障数据的完整性，但是集群的高可用不如Green。当集群状态为Yellow时，如果有一个主分片的节点出现故障，则集群将有数据缺失的风险。Yellow可以理解为需要进行故障维护的状态。

( 3) Red：至少有一个主分片运行异常。这时集群的搜索请求只能返回部分数据，而分配到这个分片上的写入请求将会返回异常。

### 数据路由规则

Elasticsearch的数据路由（Routing）规则用于确定文档存储在哪个索引（Index）的哪个分片（Shard）上。根据路由规则，ElasticSearch将不同文档索引到不同索引的不同分片上。在查询文档的时候，ElasticSearch根据路由规则找到该索引及其对应的分片并查询该文档。其路由规则的公式如下。

shard= hash(routing）%number_of_primary_shards 

在上述公式中，routing是一个可变值，默认是文档的 id，也可以设置成一个自定义的值。上述公式简述为文档所在分片等于routing的Hash值除以主分片数量(number_of_primary_shards ）的余数。这也是为什么Elasticsearch索引的主分片数量在确定后就不能再修改的原因，因为如果主分片数量发生变化，则之前路由的所有分片都会失效。

在使用时，所有API(get、index、delete、bulk、update以及mget）都接收一个叫作routing的路由参数，通过这个参数应用程序可以自定义文档到分片的映射。一个自定义的路由参数可以用来确保所有相关的文档（例如所有属于同一个用户的文档）都被存储到同一个分片中。

### 文档分片和副本策略

Elasticsearch文档分片的原则如下。

( 1 ) Elasticsearch中的每个索引都由一个或多个分片组成，文档根据路由规则分配到不同分片上.

( 2 ）每个分片都对应一个Lucene实例，一个分片只能存放Integer.MAX_VALUE -128 = 2 147 483 519个文档。

( 3 ）分片主要用于数据的横向分布，ElasticSearch中的分片会被尽可能平均地分配到不同节点上，当有新的节点加入时，ElasticSearch会自动感知并对数据进行relocation操作（例如，有2个节点，4个主分片，那么每个节点都将会分到2个分片，当再增加2个节点后，ElasticSearch会自动执行relocation操作，这时每个节点都将会分到1个分片），relocation保障了集群内数据的均衡分布.

ElasticSearch文档副本的策略如下。

( 1 ) ElasticSearch的副本即主分片（Primary Shard ）对应数据的副本分片（Replica Shard）。

( 2 ）为了防止单节点服务器故障，ElasticSearch会将主分片和副本分片分配在不同节点上。ElasticSearch的默认配置是一个索引包含5个分片，每个分片都有1个副本（即5Primary+5 Replica= 10个分片）.

## 11.ElasticSearch的写操作流程

### ElasticSeach的写操作

ElasticSeach的写操作主要包括索引的创建和删除，以及文档的创建、删除、更新等操作。ElasticSeach首先会在主分片上执行写操作，当主分片上执行成功时，根据集群的数据一致性要求，将在其他副本分片上执行写操作，只有达到一致性要求的节点都执行成功后才向客户端发送成功响应。

当向ElasticSeach发送请求时，可以将请求发送到集群种的任一节点。ElasticSeach集群中的每个节点都有能力处理任意请求。每个节点都知道集群中任一文档的位置。接收请求的节点被称为协调节点。

- 客户端选择一个 node 发送请求过去，这个 node 就是 `coordinating node`（协调节点）。
- `coordinating node` 对 document 进行**路由**，将请求转发给对应的 node（有 primary shard）。
- 实际的 node 上的 `primary shard` 处理请求，然后将数据同步到 `replica node`。
- `coordinating node` 如果发现 `primary node` 和所有 `replica node` 都搞定之后，就返回响应结果给客户端

### 数据写入一致性：Consistency

ElasticSeach通过Consistency设置写操作的一致性级级。Consistency的参数值可以是one（仅主要分片）、all（主分片和所有副本分片），或者是默认的Quorum（大部分分片）。

在默认情况下，主分片需要通过Quorum，即确认大部分副本分片有效时，才会发起一个写操作。这样做的目的是防止将数据写入网络中“错误的一侧（Wrong Side ）”。Quorum的定义如下。

quorum = int ((primary＋number_of_replicas)/2) +1 

上述公式的number_of_replicas指定在索引设置中的副本分片的数量，而不是当前处于活动状态的副本分片数量。例如，在索引中指定了有1个主分片，3个副本分片，那么Quorum的计算结果为3。

quorum = int ((primary+number_of_replicas) /2) +1＝int( (1+3) /2) +1=3 

那么当只启动了两个节点时，将永远无法满足Quorum条件，因此将无法执行写操作。

## 12.ElasticSearch的读操作流程

ElasticSearch在处理读取请求时，协调节点在每次收到客户端请求的时候都会通过轮询所有副本分片来达到负载均衡。当检索时，被索引的文档可能已经在主分片上，但是还没有同步到副本分片。在这种情况下，副本分片可能会报告文档不存在，但是主分片可能会成功返回文档。一旦索引请求成功返回用户，文挡在主分片和副本分片上都是可用的。

- 客户端发送请求到**任意**一个 node，成为 `coordinate node`。
- `coordinate node` 对 `doc id` 进行哈希路由，将请求转发到对应的 node，此时会使用 `round-robin` **随机轮询算法**，在 `primary shard` 以及其所有 replica 中随机选择一个，让读请求负载均衡。
- 接收请求的 node 返回 document 给 `coordinate node`。
- `coordinate node` 返回 document 给客户端。

## 13.ElasticSearch中的Translog 

### ElasticSearch Translog的介绍

ElasticSearch 的事务日志文件为Translog，记录了所有与更新相关的事务操作日志（例如add/update/delete）。在ElasticSearch 中，索引被分为多个分片，每个分片都对应一个Translog文件。Translog提供了数据没有被刷盘的一个持久化存储纪录，主要用来恢复数据。当ElasticSearch 重启时，它会在磁盘中使用最后一个提交点（Commit Point ）去恢复已知的段数据，并且会重放Translog中所有在最后一次提交启发生的变更操作

### ElasticSearch的数据更新和Translog操作流程

Translog被用来提供实时CRUD。当我们试着通过id查询、更新、删除一个文档时，ElasticSearch会在尝试从相应段中检索之前，首先检查Translog的最近变更记录，以保障客户端查询总是能够实时地获取文档的最新版本。

( I ）当ElasticSearch写数据时并没有直接将数据落盘，而是为了提高写入的效率将数据同时写入内存和Translog文件。

( 2 ）在refresh时间过后，将内存缓冲区的数据写入Lucene的Segment，同时清空内存缓冲区。refresh时间通过在Mapping的Setting中设置refresh_interval来实现。

( 3 ）在文件系统的fsync操作完成后，向磁盘里写入Commit Point信息。

( 4 ）删除Translog对应的日志。

下面的参数可以用来设置Translog的刷新策略。

( 1 ) index.translog.sync_interval：每间隔多久事务日志执行fsync操作和提交写操作。默认为5s，设置的值需要大于等于100ms.

( 2) index.translog.durability：是否在每次index、delete、update、bulk请求后都去执行fsync和提交事务日志的操作，具体参数设置如下。

request：每次请求均执行fsync和提交操作。它可保障发生硬件故障时，所有确认的写操作都将被写入磁盘。

sync：每隔sync_interval时间将会执行fsync和提交操作。当发生硬件故障时，所有确认的写操作在上次自动提交后都会被丢弃。

( 3) index.translog.flush_threshold_size：当事务日志中存储的数据大于该值时，则启动刷新操作，产生一个新的Commit Point。默认值为512MB。

( 4) index.translog.retention.size：保留事务日志的总大小。当恢复备份数据时，太多事务日志文件将增大基于sync操作的概率；如果事务日志不够，则备份恢复将会回退到基于sync的文件。默认值为512MB，在ElasticSearch 7.0.0及更高版本中，该配置已经被废弃。

( 5 ) index.translog.retention.age：事务日志保留的最长时间，默认为12h，在ElasticSearch 7.0.0及更高版本中，该配置已经被废弃。

## 14.ElasticSearch段合并

### 段合并的介绍

由于自动刷新流程每秒都会创建一个新的段，这样会导致短时间内段的数量迅速增加。而段的数量太多将会引起性能瓶颈。在ElasticSearch中，每一个段都会消耗文件句柄、内存和CPU运行周期。同时，每个搜索请求都必须轮流检查每个段；因此段越多，搜索也就越慢。ElasticSearch通过在后台进行段合并来解决这个问题。小的段被合并到大的段，大的段再被合并到更大的段。

在段合并的时候会将那些旧的已删除文档从文件系统中清除。被删除的文档（或被更新文档的旧版本）不会被复制到新的大段中。段合并在进行索引和搜索时会自动进行。

### 段台并的流程

将两个提交了的段和一个未提交的段合并到一个更大的段。

( 1 ）在索引的时候，Refresh操作会创建新的段并将段打开以供搜索使用。

( 2 ）合并进程选择一小部分大小相似的段，并且在后台将它们合并到更大的段中。该操作并不会中断索引和搜索。

( 3 ）在合并结束后，老的段被删除，新的段被Flush到磁盘（写入一个包含新段且排除旧段和较小段的新提交点），再打开新的段对外提供搜索，删除老的段数据。

合并大的段需要消耗大量的I/O和CPU资源，ElasticSearch对合并流程进行资源限制，以保障搜索仍然有足够的资源被正常地执行。

可以通过API设置max_bytes_per_sec来提高段合并的性能，默认值为20MB/s，对于机械磁盘，20MB/s是合理的设置，但如果磁盘是SSD，则考虑提高到100~200MB/s。

```
PUT /_cluster/settings 
{
	"persistent":{
		"indices.store.throttle.max_bytes_per_sec":”lOOmb”
	}
}
```

可以通过以下API彻底关掉合并限流，使段合并速度尽可能快，上限为磁盘允许的最大读写速度。

```
PUT /_cluster/settings 
{
	"transient”：{
		”indices.store.throttle.type ”:”none"
	}
}
```

 同时，可以在elasticsearch.yml配置中设置max_thread_ count的个数以提高段合并的并发度。

index.merge.scheduler.max_thread_count: 10 

## 15.ElasticSearch的集群扩容

ElasticSearch的扩容分为垂直扩容和水平扩容两种方式。垂直扩容指加大现有节点的内存、CPU和磁盘等资源。水平扩容指向集群中新加个一个节点（一般与当前节点配置相同）。ElasticSearch一般采用水平扩容的方式进行扩容。

实践证明，在集群总体资源一定的情况下，使用内存容量小、性能相对较低、节点较多的部署方案，其性能往往优于使用内存容量大、性能相对较高、节点较少的部署方案。

ElasticSearch拥有集群发现（Cluster Discovery）机制，在启动一个新的节点后，该节点会发现集群并自动加入集群，ElasticSearch集群会自动在各个分片之间执行数据的均衡处理。

## 16.ElasticSearch的应用

ElasticSearch广泛应用于大数据存储、日志分析、运维监控等多种场景，支持单机部署和集群部署两种方式。在API使用层面，ElasticSearch支持Java API和REST API两种方式。

## 17.ElasticSearch的配置和性能调优

### JVM性能调优

ElasticSearch基于Java实现，默认使用的堆内存为1GB，对于生产环境需要根据系统资源对堆内存进行合理的设置以达到良好的性能表现。执行以下命令对JVM堆内存进行设置。

vim config/jvm.options 

如果操作系统有32GB内存，则建议将JVM堆内存的最小值和最大值都设置为16GB。

-Xms16gb 

-Xmx16gb 

这里将堆内存最小值（Xms）与最大值（Xmx）设置相同，防止在ElasticSearch运行过程中JVM改变堆内存大小，引起JVM内存震荡。

需要注意的是，ElasticSearch除了使用JVM堆内存，其内部Lucene还需要使用大量非堆内存。ElasticSearch内部使用Lucene实现全文检索。Lucene的段分别存储在单个文件中，因为段是不可变的，对缓存友好的，所以在使用段数据时操作系统会把这些段文件缓存起来，以便更快地访问。同时，Lucene可以利用操作系统底层机制来缓存内存数据，加速查询效率。

Lucene的性能取决于与操作系统交互的速度，而这些交互都需要大量的内存资源（非JVM堆内存），如果把全部内存都分配给JVM堆内存，则将导致Lucene在运行过程中因资源不足而性能下降。一般建议将系统的一半内存分配给JVM堆内存，另外一半内存预留给Lucene和操作系统。比如有32GB内存，可以把16GB分配给JVM堆内存，剩余的16GB预留给Lucene和操作系统。

### 操作系统的性能调优

( 1）设置文件句柄：Linux中的每个进程默认打开的最大文件句柄数都是1024，对于服务器进程来说该值太小，可以通过修改/etc/security/limits.conf来增大打开的最大文件句柄数，一般建议设置为65535。设置命令如下。

echo "* soft nofile 65535”>>/etc/security/limits.conf

echo ”* hard nofile 65535”>>/etc/security/limits.conf

( 2 ）设置虚拟内存：max_map_ count定义了进程能拥有的最多内存区域，一般建议设置为102400。设置命令如下。

sysctl -w vm.max_map_count=102400 

( 3 ）关闭Swap：Swap空间是一块磁盘空间，操作系统使用这块空间保存从内存中交互换出的操作系统不常用的Page数据，这样可以分配出更多的内存做Page Cache。通过Swap可以提升系统的吞吐量和I/O性能，但ElasticSearch需要一个所有内存操作都能够被快速执行的环境，服务一旦使用到了Swap内存，就会大大降低数据的存取效率，严重影响性能。关闭设置的命令如下。

echo "vm.swappiness ＝0">>／etc/sysctl.conf

( 4 ）开启mlockall：打开配置文件中的mlockall开关。它的作用是允许JVM锁住内存，禁止操作系统将内存交换出去。elasticsearch.yml文件中的设置如下。

bootstrap.mlockall: true 

## 18.es 核心概念 vs. db 核心概念

| es       | db       |
| -------- | -------- |
| index    | 数据库   |
| type     | 数据表   |
| docuemnt | 一行数据 |

## 19.es 搜索数据过程

es 最强大的是做全文检索，就是比如你有三条数据：

```html
 java真好玩儿啊
 java好难学啊
 j2ee特别牛
```

你根据 `java` 关键词来搜索，将包含 `java`的 `document` 给搜索出来。es 就会给你返回：java真好玩儿啊，java好难学啊。

- 客户端发送请求到一个 `coordinate node`。
- 协调节点将搜索请求转发到**所有**的 shard 对应的 `primary shard` 或 `replica shard`，都可以。
- query phase：每个 shard 将自己的搜索结果（其实就是一些 `doc id`）返回给协调节点，由协调节点进行数据的合并、排序、分页等操作，产出最终结果。
- fetch phase：接着由协调节点根据 `doc id` 去各个节点上**拉取实际**的 `document` 数据，最终返回给客户端。

> 写请求是写入 primary shard，然后同步给所有的 replica shard；读请求可以从 primary shard 或 replica shard 读取，采用的是随机轮询算法。

## 20.写数据底层原理

先写入内存 buffer，在 buffer 里的时候数据是搜索不到的；同时将数据写入 translog 日志文件。

如果 buffer 快满了，或者到一定时间，就会将内存 buffer 数据 `refresh` 到一个新的 `segment file` 中，但是此时数据不是直接进入 `segment file` 磁盘文件，而是先进入 `os cache` 。这个过程就是 `refresh`。

每隔 1 秒钟，es 将 buffer 中的数据写入一个**新的** `segment file`，每秒钟会产生一个**新的磁盘文件** `segment file`，这个 `segment file` 中就存储最近 1 秒内 buffer 中写入的数据。

但是如果 buffer 里面此时没有数据，那当然不会执行 refresh 操作，如果 buffer 里面有数据，默认 1 秒钟执行一次 refresh 操作，刷入一个新的 segment file 中。

操作系统里面，磁盘文件其实都有一个东西，叫做 `os cache`，即操作系统缓存，就是说数据写入磁盘文件之前，会先进入 `os cache`，先进入操作系统级别的一个内存缓存中去。只要 `buffer` 中的数据被 refresh 操作刷入 `os cache`中，这个数据就可以被搜索到了。

为什么叫 es 是**准实时**的？ `NRT`，全称 `near real-time`。默认是每隔 1 秒 refresh 一次的，所以 es 是准实时的，因为写入的数据 1 秒之后才能被看到。可以通过 es 的 `restful api` 或者 `java api`，**手动**执行一次 refresh 操作，就是手动将 buffer 中的数据刷入 `os cache`中，让数据立马就可以被搜索到。只要数据被输入 `os cache` 中，buffer 就会被清空了，因为不需要保留 buffer 了，数据在 translog 里面已经持久化到磁盘去一份了。

重复上面的步骤，新的数据不断进入 buffer 和 translog，不断将 `buffer` 数据写入一个又一个新的 `segment file` 中去，每次 `refresh` 完 buffer 清空，translog 保留。随着这个过程推进，translog 会变得越来越大。当 translog 达到一定长度的时候，就会触发 `commit` 操作。

commit 操作发生第一步，就是将 buffer 中现有数据 `refresh` 到 `os cache` 中去，清空 buffer。然后，将一个 `commit point` 写入磁盘文件，里面标识着这个 `commit point` 对应的所有 `segment file`，同时强行将 `os cache` 中目前所有的数据都 `fsync` 到磁盘文件中去。最后**清空** 现有 translog 日志文件，重启一个 translog，此时 commit 操作完成。

这个 commit 操作叫做 `flush`。默认 30 分钟自动执行一次 `flush`，但如果 translog 过大，也会触发 `flush`。flush 操作就对应着 commit 的全过程，我们可以通过 es api，手动执行 flush 操作，手动将 os cache 中的数据 fsync 强刷到磁盘上去。

translog 日志文件的作用是什么？你执行 commit 操作之前，数据要么是停留在 buffer 中，要么是停留在 os cache 中，无论是 buffer 还是 os cache 都是内存，一旦这台机器死了，内存中的数据就全丢了。所以需要将数据对应的操作写入一个专门的日志文件 `translog` 中，一旦此时机器宕机，再次重启的时候，es 会自动读取 translog 日志文件中的数据，恢复到内存 buffer 和 os cache 中去。

translog 其实也是先写入 os cache 的，默认每隔 5 秒刷一次到磁盘中去，所以默认情况下，可能有 5 秒的数据会仅仅停留在 buffer 或者 translog 文件的 os cache 中，如果此时机器挂了，会**丢失** 5 秒钟的数据。但是这样性能比较好，最多丢 5 秒的数据。也可以将 translog 设置成每次写操作必须是直接 `fsync` 到磁盘，但是性能会差很多。

实际上你在这里，如果面试官没有问你 es 丢数据的问题，你可以在这里给面试官炫一把，你说，其实 es 第一是准实时的，数据写入 1 秒后可以搜索到；可能会丢失数据的。有 5 秒的数据，停留在 buffer、translog os cache、segment file os cache 中，而不在磁盘上，此时如果宕机，会导致 5 秒的**数据丢失**。

### 总结

数据先写入内存 buffer，然后每隔 1s，将数据 refresh 到 os cache，到了 os cache 数据就能被搜索到（所以我们才说 es 从写入到能被搜索到，中间有 1s 的延迟）。每隔 5s，将数据写入 translog 文件（这样如果机器宕机，内存数据全没，最多会有 5s 的数据丢失），translog 大到一定程度，或者默认每隔 30mins，会触发 commit 操作，将缓冲区的数据都 flush 到 segment file 磁盘文件中。

> 数据写入 segment file 之后，同时就建立好了倒排索引。

## 21.删除/更新数据底层原理

如果是删除操作，commit 的时候会生成一个 `.del` 文件，里面将某个 doc 标识为 `deleted` 状态，那么搜索的时候根据 `.del` 文件就知道这个 doc 是否被删除了。

如果是更新操作，就是将原来的 doc 标识为 `deleted` 状态，然后新写入一条数据。

buffer 每 refresh 一次，就会产生一个 `segment file`，所以默认情况下是 1 秒钟一个 `segment file`，这样下来 `segment file` 会越来越多，此时会定期执行 merge。每次 merge 的时候，会将多个 `segment file` 合并成一个，同时这里会将标识为 `deleted` 的 doc 给**物理删除掉**，然后将新的 `segment file` 写入磁盘，这里会写一个 `commit point`，标识所有新的 `segment file`，然后打开 `segment file` 供搜索使用，同时删除旧的 `segment file`。

## 22.es 在数据量很大的情况下（数十亿级别）如何提高查询效率啊？

### 性能优化的杀手锏——filesystem cache

你往 es 里写的数据，实际上都写到磁盘文件里去了，**查询的时候**，操作系统会将磁盘文件里的数据自动缓存到 `filesystem cache` 里面去。

es 的搜索引擎严重依赖于底层的 `filesystem cache`，你如果给 `filesystem cache` 更多的内存，尽量让内存可以容纳所有的 `idx segment file`索引数据文件，那么你搜索的时候就基本都是走内存的，性能会非常高。

性能差距究竟可以有多大？我们之前很多的测试和压测，如果走磁盘一般肯定上秒，搜索性能绝对是秒级别的，1秒、5秒、10秒。但如果是走 `filesystem cache`，是走纯内存的，那么一般来说性能比走磁盘要高一个数量级，基本上就是毫秒级的，从几毫秒到几百毫秒不等。

归根结底，你要让 es 性能要好，最佳的情况下，就是你的机器的内存，至少可以容纳你的总数据量的一半。

根据我们自己的生产环境实践经验，最佳的情况下，是仅仅在 es 中就存少量的数据，就是你要**用来搜索的那些索引**，如果内存留给 `filesystem cache` 的是 100G，那么你就将索引数据控制在 `100G` 以内，这样的话，你的数据几乎全部走内存来搜索，性能非常之高，一般可以在 1 秒以内。

比如说你现在有一行数据。`id,name,age ....` 30 个字段。但是你现在搜索，只需要根据 `id,name,age` 三个字段来搜索。如果你傻乎乎往 es 里写入一行数据所有的字段，就会导致说 `90%` 的数据是不用来搜索的，结果硬是占据了 es 机器上的 `filesystem cache` 的空间，单条数据的数据量越大，就会导致 `filesystem cahce` 能缓存的数据就越少。其实，仅仅写入 es 中要用来检索的**少数几个字段**就可以了，比如说就写入 es `id,name,age` 三个字段，然后你可以把其他的字段数据存在 mysql/hbase 里，我们一般是建议用 `es + hbase` 这么一个架构。

hbase 的特点是**适用于海量数据的在线存储**，就是对 hbase 可以写入海量数据，但是不要做复杂的搜索，做很简单的一些根据 id 或者范围进行查询的这么一个操作就可以了。从 es 中根据 name 和 age 去搜索，拿到的结果可能就 20 个 `doc id`，然后根据 `doc id` 到 hbase 里去查询每个 `doc id` 对应的**完整的数据**，给查出来，再返回给前端。

写入 es 的数据最好小于等于，或者是略微大于 es 的 filesystem cache 的内存容量。然后你从 es 检索可能就花费 20ms，然后再根据 es 返回的 id 去 hbase 里查询，查 20 条数据，可能也就耗费个 30ms，可能你原来那么玩儿，1T 数据都放 es，会每次查询都是 5~10s，现在可能性能就会很高，每次查询就是 50ms。

### 数据预热

假如说，哪怕是你就按照上述的方案去做了，es 集群中每个机器写入的数据量还是超过了 `filesystem cache` 一倍，比如说你写入一台机器 60G 数据，结果 `filesystem cache` 就 30G，还是有 30G 数据留在了磁盘上。

其实可以做**数据预热**。

举个例子，拿微博来说，你可以把一些大V，平时看的人很多的数据，你自己提前后台搞个系统，每隔一会儿，自己的后台系统去搜索一下热数据，刷到 `filesystem cache` 里去，后面用户实际上来看这个热数据的时候，他们就是直接从内存里搜索了，很快。

或者是电商，你可以将平时查看最多的一些商品，比如说 iphone 8，热数据提前后台搞个程序，每隔 1 分钟自己主动访问一次，刷到 `filesystem cache` 里去。

对于那些你觉得比较热的、经常会有人访问的数据，最好**做一个专门的缓存预热子系统**，就是对热数据每隔一段时间，就提前访问一下，让数据进入 `filesystem cache` 里面去。这样下次别人访问的时候，性能一定会好很多。

### 冷热分离

es 可以做类似于 mysql 的水平拆分，就是说将大量的访问很少、频率很低的数据，单独写一个索引，然后将访问很频繁的热数据单独写一个索引。最好是将**冷数据写入一个索引中，然后热数据写入另外一个索引中**，这样可以确保热数据在被预热之后，尽量都让他们留在 `filesystem os cache` 里，**别让冷数据给冲刷掉**。

你看，假设你有 6 台机器，2 个索引，一个放冷数据，一个放热数据，每个索引 3 个 shard。3 台机器放热数据 index，另外 3 台机器放冷数据 index。然后这样的话，你大量的时间是在访问热数据 index，热数据可能就占总数据量的 10%，此时数据量很少，几乎全都保留在 `filesystem cache` 里面了，就可以确保热数据的访问性能是很高的。但是对于冷数据而言，是在别的 index 里的，跟热数据 index 不在相同的机器上，大家互相之间都没什么联系了。如果有人访问冷数据，可能大量数据是在磁盘上的，此时性能差点，就 10% 的人去访问冷数据，90% 的人在访问热数据，也无所谓了。

### document 模型设计

对于 MySQL，我们经常有一些复杂的关联查询。在 es 里该怎么玩儿，es 里面的复杂的关联查询尽量别用，一旦用了性能一般都不太好。

最好是先在 Java 系统里就完成关联，将关联好的数据直接写入 es 中。搜索的时候，就不需要利用 es 的搜索语法来完成 join 之类的关联搜索了。

document 模型设计是非常重要的，很多操作，不要在搜索的时候才想去执行各种复杂的乱七八糟的操作。es 能支持的操作就那么多，不要考虑用 es 做一些它不好操作的事情。如果真的有那种操作，尽量在 document 模型设计的时候，写入的时候就完成。另外对于一些太复杂的操作，比如 join/nested/parent-child 搜索都要尽量避免，性能都很差的。

### 分页性能优化

es 的分页是较坑的，为啥呢？举个例子吧，假如你每页是 10 条数据，你现在要查询第 100 页，实际上是会把每个 shard 上存储的前 1000 条数据都查到一个协调节点上，如果你有个 5 个 shard，那么就有 5000 条数据，接着协调节点对这 5000 条数据进行一些合并、处理，再获取到最终第 100 页的 10 条数据。

分布式的，你要查第 100 页的 10 条数据，不可能说从 5 个 shard，每个 shard 就查 2 条数据，最后到协调节点合并成 10 条数据吧？你**必须**得从每个 shard 都查 1000 条数据过来，然后根据你的需求进行排序、筛选等等操作，最后再次分页，拿到里面第 100 页的数据。你翻页的时候，翻的越深，每个 shard 返回的数据就越多，而且协调节点处理的时间越长，非常坑爹。所以用 es 做分页的时候，你会发现越翻到后面，就越是慢。

我们之前也是遇到过这个问题，用 es 作分页，前几页就几十毫秒，翻到 10 页或者几十页的时候，基本上就要 5~10 秒才能查出来一页数据了。

有什么解决方案吗？

**不允许深度分页（默认深度分页性能很差）**

跟产品经理说，你系统不允许翻那么深的页，默认翻的越深，性能就越差。

类似于 app 里的推荐商品不断下拉出来一页一页的

类似于微博中，下拉刷微博，刷出来一页一页的，你可以用 `scroll api`，关于如何使用，自行上网搜索。

scroll 会一次性给你生成**所有数据的一个快照**，然后每次滑动向后翻页就是通过**游标** `scroll_id` 移动，获取下一页下一页这样子，性能会比上面说的那种分页性能要高很多很多，基本上都是毫秒级的。

但是，唯一的一点就是，这个适合于那种类似微博下拉翻页的，**不能随意跳到任何一页的场景**。也就是说，你不能先进入第 10 页，然后去第 120 页，然后又回到第 58 页，不能随意乱跳页。所以现在很多产品，都是不允许你随意翻页的，app，也有一些网站，做的就是你只能往下拉，一页一页的翻。

初始化时必须指定 `scroll` 参数，告诉 es 要保存此次搜索的上下文多长时间。你需要确保用户不会持续不断翻页翻几个小时，否则可能因为超时而失败。

除了用 `scroll api`，你也可以用 `search_after` 来做，`search_after` 的思想是使用前一页的结果来帮助检索下一页的数据，显然，这种方式也不允许你随意翻页，你只能一页页往后翻。初始化时，需要使用一个唯一值的字段作为 sort 字段。

## 23.es 生产集群的部署架构是什么？每个索引的数据量大概有多少？每个索引大概有多少个分片？

一个基本的版本，你到时候就简单说一下就好了。

- es 生产集群我们部署了 5 台机器，每台机器是 6 核 64G 的，集群总内存是 320G。
- 我们 es 集群的日增量数据大概是 2000 万条，每天日增量数据大概是 500MB，每月增量数据大概是 6 亿，15G。目前系统已经运行了几个月，现在 es 集群里数据总量大概是 100G 左右。
- 目前线上有 5 个索引（这个结合你们自己业务来，看看自己有哪些数据可以放 es 的），每个索引的数据量大概是 20G，所以这个数据量之内，我们每个索引分配的是 8 个 shard，比默认的 5 个 shard 多了 3 个 shard。