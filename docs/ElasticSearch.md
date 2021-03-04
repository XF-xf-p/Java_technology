## 1.ElasticSearch

ElasticSearch是一个分布式、基于RESTful风格的数据搜索和分析引擎，由Elastic公司开发并基于Apache许可条款发布源码。ElasticSearch的底层全文检索基于Lucene实现，其灵活的数据存取和分析方式、良好的性能和稳定性使其在大数据存储和分析领域被广泛使用。

## 2.Lucene简介

ElasticSearch的底层存储基于Lucene实现，Lucene是Apache软件基金的一个开源子项目，是一套全文检索引擎架构，提供了完整的文本分析引擎、数据查询引擎和数据索引引擎。Lucene的目的是为软件开发人员提供一个简单易用的工具包，以便在目标系统中实现全文检索的功能，或者以Lecene为基础建立一套完整的全文检索引擎。

## 3.倒排排索

在实际应用中，我们常常需要根据属性的值来查找记录，这时就需要使用倒排索引( Inverted Index）。倒排索引表中的每一项都包括一个属性值和具有该属性值对应记录的地址。由于不是按照记录来确定属性值的，而是由属性值来确定记录的位置的，因此被称为倒排索引。例如，当我们在百度搜索栏中输入关键词时，百度会按照输入的关键词在所有文档内容（比如网页内容）中搜索与关键词相关的记录（比如网站），并将内容相关的记录的地址（比如网站的地址）返回用户，然后用户便可按该记录的地址进一步查看记录的详细信息。

搜索引擎的关键是建立倒排索引，倒排索引一般表示一个关键词，以及它的频度（出现的次数）和位置（出现在哪一篇文章或网页中，以及相关的日期、作者等信息）。

带有倒排索引的文件被称为倒排索引文件（Inverted File）。 倒排索引的索引对象是文档或者文档集合中的单词，倒排索引文件被用来存储这些单词在一个文档或者一组文档中的位置。

## 4.Lucene的架构

Lucene是一个高并发、高吞吐、可扩展的全文检索库。它基于Java实现，使用方便。Lucene内部的数据结构叫作文档（Document），当应用层的数据（例如，FieSystem、Web Data、DataBase等）进入Lucene时，首先会进行索引文档（Index Document ）操作，按照索引规则创建倒排索引；在应用程序查询数据的时候，直接查询提建好的倒排索引，因此其效率十分高。

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

( 1 ）客户端向Node-1发送新建、查询或者删除文档的请求。节点根据文档的id为1确定文档属于分片1.

( 2 ）因为分片1的主分片P-1被分配在Node-3上，所以请求会被转发到Node-3。

( 3 ）在Node-3的主分片上执行请求，如果执行成功，则将请求同时转发到Node-1和Node-2的副本分片R-1上执行。

(4）当所有副本分片都报告执行成功时，Node-3才向协调节点报告执行成功.

(5）协调节点向客户端报告成功。当客户端收到成功响应时，文档更新已经在主分片和所有副本分片上都执行成功。

### 数据写入一致性：Consistency

ElasticSeach通过Consistency设置写操作的一致性级级。Consistency的参数值可以是one（仅主要分片）、all（主分片和所有副本分片），或者是默认的Quorum（大部分分片）。

在默认情况下，主分片需要通过Quorum，即确认大部分副本分片有效时，才会发起一个写操作。这样做的目的是防止将数据写入网络中“错误的一侧（Wrong Side ）”。Quorum的定义如下。

quorum = int ((primary＋number_of_replicas)/2) +1 

上述公式的number_of_replicas指定在索引设置中的副本分片的数量，而不是当前处于活动状态的副本分片数量。例如，在索引中指定了有1个主分片，3个副本分片，那么Quorum的计算结果为3。

quorum = int ((primary+number_of_replicas) /2) +1＝int( (1+3) /2) +1=3 

那么当只启动了两个节点时，将永远无法满足Quorum条件，因此将无法执行写操作。

## 12.ElasticSearch的读操作流程

ElasticSearch在处理读取请求时，协调节点在每次收到客户端请求的时候都会通过轮询所有副本分片来达到负载均衡。当检索时，被索引的文档可能已经在主分片上，但是还没有同步到副本分片。在这种情况下，副本分片可能会报告文档不存在，但是主分片可能会成功返回文档。一旦索引请求成功返回用户，文挡在主分片和副本分片上都是可用的。

( 1 ）客户端向Node-1发送文档读取请求。

( 2 ）协调节点Node-1根据文档的id来确定文档属于分片1。分片1的文档数据存在所有3个节点上。在这种情况下，它将请求转发到Node-2。

( 3 ) Node-2在本地执行查询操作并将查询结果返回到Node-1。

( 4 ) Node-1（此时Node-1为CoordinatingNode角色）接收Node-2的查询结果，如果查询到请求对应的文档，则将该文档返回客户端。如果在Node-2上未查询到对应的文档数据，则Node-1会继续向其他节点发送文档读取请求，直到查询到文档对应的数据后才返回。如果要读取的文档在所有节点上都不存在，则向客户端报告文档不存在。

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

