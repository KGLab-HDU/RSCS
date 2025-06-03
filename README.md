

# Relational Similarity-Guided Community Search on Semantic-Rich Graphs

## 1 Introduction

This is a description of the code used for the experiments described in the paper entitled *Relational Similarity-Guided Community Search on Semantic-Rich Graphs*. 



We evaluated our **R**elational **S**imilarity-Guided **C**ommunity **S**earch on Semantic-Rich Graphs (RSCS) in terms of effectiveness and efficiency for $(k,\textsf{SMP})$-core on semantic-rich graphs. We provide three versions of our own methods, see Table 1.

**Table** 1 **Our Methods**

| ALGORITHM  | DESCRIPTION                                                  |
| ---------- | ------------------------------------------------------------ |
| E+Refine   | The basic algorithm proposed in our paper, which involves two parts: <u>E</u>xpansion-based $G_{\rm SMP}$ generation and community refinement (shorten as Refine). |
| RE+Refine  | RE+Refine is the first optimized algorithm with <u>R</u>ecord-based <u>E</u>xpansion and original Refine. |
| GSE+Refine | GSE+Refine is the second optimized algorithm with <u>G</u>reedy <u>S</u>earch-based <u>E</u>xpansion and original Refine. |

## 2 Requirements

The experiments have been run on a Linux server with a 3.7 GHZ, 128 GB memory. All programs are developed using Java 17 and don't need any incorporate additional JAR packages.

## 3 DATASETS

Our experiment involves five *datasets* popularly deployed by existing works. Each dataset represents a  hetergeneous graph with rich semantics.

The hetergeneous graphs contain graph files, node files, relation files and similarity files, which are stored in the form of txt text. 

Here, DBpedia is taken as an example. 

- node.txt: each line of the node file represents the type of the node, in the form of *dot-blank-dot*.

| node_id | node type |
| :-----: | :-------: |
| 465882  |    336    |

- relation.txt: each line of the relation file represents the ID of the relation except for the first line, in the form of *dot-blank-dot*. The first line of the file records the total number **x** of relations.

| relation name | relation id |
| :-----------: | :---------: |
|   assembly    |     56      |

- similarity.txt: The similarity file recoreds semantic similarity of all relation, organized in grouped segments. Each segment begins with a reference relation in its first line, followed by **x** subsequent lines containing semantic similarity between this reference relation and other relations, in the form of *dot-blank-dot*, sorted in descending order of semantic similarity.

| relation name | semantic similarity |
| :-----------: | :-----------------: |
|    country    |       0.9899        |

- graph.txt: Each row of the graph file represents information of all adjacent nodes and adjacent relations of a node in the form of *source node-blank-adjacent relation 1-blank-adjacent node_id 1-blank-blank-adjacent relation 2-blank-adjacent node_id 2*.

| source node id | adjacent relation 1 | adjacent node id 1 | adjacent relation 2 | adjacent node id 2 |
| :------------: | :-----------------: | :----------------: | :-----------------: | :----------------: |
|    7053036     |      assembly       |       419963       |      assembly       |      3003510       |

## 4 Usage

#### 4.1 Efficiency

We discuss how to obtain the running time of our method. Our proposed methods employ identical parameters: a *relation file path*, a *similarity file path*, a *graph file path*, a *node type file path*, a *query relation*, a *type id* of anchor node, a length bound *l*, a query *k*, and a threshold $\tau$. We use GSE+Refine as an example, other methods can be invoked as the same way. To execute  E+Refine, invoke **test.Efficiency.ETest**; to execute RE+Refine, call **test.Efficiency.RETest**.

```
GSE: 
relationFilePath=""
similarityFilePath=""
graphFilePath=""
vertexTypeFilePath=""
queryRelation=""
AaType=""
queryID=""
l=""
queryK=""
tao=""
java -jar RSCS-main.jar test.Efficiency.GSETest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao}
```

We provide a reduced *DBpedia* for testing with the following experimental parameters, and redirect the output to test.txt. All required files are stored in the **data** folder.

```
GSE: 
relationFilePath="./data/relation.txt"
similarityFilePath="./data/similarity.txt"
graphFilePath="./data/graph.txt"
vertexTypeFilePath="./data/node.txt"
queryRelation="assembly"
AaType="346"
queryID="465882"
l="3"
queryK="10"
tao="0.93"
java -jar RSCS-main.jar test.Efficiency.GSETest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao}>test.txt 2>&1 &
```

Output

we output the following results, including running time, generate $G_{\textsf{smp}}$ time, refine time, community size and community nodes, respectively. Here is the output of GSE + Refine for the above query example:

```
Output of "
relationFilePath="./data/relation.txt"
similarityFilePath="./data/similarity.txt"
graphFilePath="./data/graph.txt"
vertexTypeFilePath="./data/node.txt"
queryRelation="assembly"
AaType="346"
queryID="465882"
l="3"
queryK="10"
tao="0.93"
java -jar RSCS-main.jar test.Efficiency.GSETest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao}>test.txt 2>&1 &":

Method: GSE
AaType:346 l:3 queryK:10 tao:0.93
data read over
start running
run over
query time:5195.7617ms
generate Gsmp time:3915.634ms
refine time:1280.1277ms
community size:3776
community nodes:[458758, 2490405, 4964431, 2277414, 7028846, ...]
```



#### 4.2 Effectiveness

We discuss how to obtain the three most important metrics: SC, TSD, and #MP. The parameters used by the Effectiveness method are the same as those used in **4.1 Efficiency**. Here is an example of parameter input.

```
Effectiveness: 
relationFilePath=""
similarityFilePath=""
graphFilePath=""
vertexTypeFilePath=""
queryRelation=""
AaType=""
queryID=""
l=""
queryK=""
tao=""
java -jar RSCS-main.jar test.Effectiveness.EffectivenessTest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao}
```

We still use the reduced *DBpedia* for testing with the following experimental parameters, and redirect the output to test.txt. All required files are stored in the **data** folder.

```
Effectiveness: 
relationFilePath="./data/relation.txt"
similarityFilePath="./data/similarity.txt"
graphFilePath="./data/graph.txt"
vertexTypeFilePath="./data/node.txt"
queryRelation="assembly"
AaType="346"
queryID="465882"
l="3"
queryK="10"
tao="0.93"
java -jar RSCS-main.jar test.Effectiveness.EffectivenessTest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao}>test.txt 2>&1 &
```

Output

we output the following results, including SC, TSD, #MP, community size and community nodes, respectively. Here is the output for the above query example:

```
Output of "
relationFilePath="./data/relation.txt"
similarityFilePath="./data/similarity.txt"
graphFilePath="./data/graph.txt"
vertexTypeFilePath="./data/node.txt"
queryRelation="assembly"
AaType="346"
queryID="465882"
l="3"
queryK="10"
tao="0.93"
java -jar RSCS-main.jar test.Effectiveness.EffectivenessTest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao}>test.txt 2>&1 &":

Effectiveness
AaType:346 l:3 queryK:10 tao:0.93
data read over
start running
run over
SC: 0.996655
TSD: 2111.0900000307424
#MP: 13
community size:3777
community nodes:[458758, 2490405, 4964431, 2277414, 7028846, ...]
```



#### 4.3 Case

We discuss how to obtain all relevant information for the case study, including the node set and the P\*edge set. The node set contains three types of information: node ID, the name of the entity corresponding to the node ID, and the entity category (label). The P\*-edge set contains four types of information: the starting node, the ending node, the -P\*edge, and the SC of the -P\*edge. These pieces of information constitute the case study in the paper.

Case methods employ identical parameters: a *relation file path*, a *similarity file path*, a *graph file path*, a *node type file path*, a *query relation*, a *type id* of anchor node, a length bound *l*, a query *k*, a threshold $\tau$, a *result of node set file path*, a *result* P\*-*edge set file path*, an *entity file path*. Here is an example of parameter input.

```
Case: 
relationFilePath=""
similarityFilePath=""
graphFilePath=""
vertexTypeFilePath=""
queryRelation=""
AaType=""
queryID=""
l=""
queryK=""
tao=""
writeNodeFilePath = ""
writeEdgeFilePath = ""
entityFilePath = ""
java -jar RSCS-main.jar test.Case.CaseTest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao} ${writeNodeFilePath} ${writeEdgeFilePath} ${entityFilePath}
```

We still use the reduced *DBpedia* for testing with the following experimental parameters, and redirect the output of console to test.txt. It should be noted that the node set and the P\*-edge set are stored separately in different designated files. All required files are stored in the **data** folder.

```
Case: 
relationFilePath="./data/relation.txt"
similarityFilePath="./data/similarity.txt"
graphFilePath="./data/graph.txt"
vertexTypeFilePath="./data/node.txt"
queryRelation="assembly"
AaType="346"
queryID="465882"
l="3"
queryK="10"
tao="0.93"
writeNodeFilePath = "./data/reNodes.txt"
writeEdgeFilePath = "./data/reEdges.txt"
entityFilePath = "./data/Entity.txt"
java -jar RSCS-main.jar test.Case.CaseTest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao} ${writeNodeFilePath} ${writeEdgeFilePath} ${entityFilePath}>test.txt 2>&1 &
```

Output

Here is the output of console for the above query example:

```
Output of "
relationFilePath="./data/relation.txt"
similarityFilePath="./data/similarity.txt"
graphFilePath="./data/graph.txt"
vertexTypeFilePath="./data/node.txt"
queryRelation="assembly"
AaType="346"
queryID="465882"
l="3"
queryK="10"
tao="0.93"
writeNodeFilePath = "./data/reNodes.txt"
writeEdgeFilePath = "./data/reEdges.txt"
entityFilePath = "./data/Entity.txt"
java -jar RSCS-main.jar test.Case.CaseTest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao} ${writeNodeFilePath} ${writeEdgeFilePath} ${entityFilePath}>test.txt 2>&1 &":

Case
AaType:346 l:3 queryK:10 tao:0.93
data read over
run over
vertex write over
edge write over
community size:3777
community nodes:[458758, 2490405, 4964431, 2277414, 7028846, ...]
```

Here is the output of node set for the above query example:

```
id	name	label
458758	Sisu_KB-124	Automobile
2490405	Honda_Integra__Series_DA5-DA9,_DB1-DB2__1	Automobile
4964431	Lotus_Carlton	Automobile
...
```

Here is the output of P\*-edge set for the above query example:

```
source	target	label	weight
458758	6787047	[458758-assembly-2828186-assembly-6787047]0.999999	0.999999
458758	1316821	[458758-assembly-2828186-country-7005850-assembly-1316821]0.996655	0.996655
458758	1316822	[458758-assembly-2828186-country-7005850-assembly-1316822]0.996655	0.996655
...
```



