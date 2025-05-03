

# Relational Similarity-Guided Community Search on Semantic-Rich Graphs

## 1 Introduction

This is a description of the code used for the experiments described in the paper entitled *Relational Similarity-Guided Community Search on Semantic-Rich Graphs*. The code is available at [4open.science](https://anonymous.4open.science/r/SEA-9BE2/README.md).

$\color{red}{链接未改！}$

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

Our proposed methods employ identical parameters: a *relation file path*, a similarity file path, a *graph file path*, a *node type file path*, a *query relation*, a *type id* of anchor node, a length bound *l*, a query *k*, and a threshold $\tau$. We use GSE+Refine as an example, other methods can be invoked as the same way. To execute  E+Refine, invoke **test.ETest**; to execute RE+Refine, call **test.RETest**.

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
java -jar RSCS-main.jar test.GSETest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao}
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
java -jar RSCS-main.jar test.GSETest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao}>test.txt 2>&1 &
```

Output

we output the following results, including semantic cohesiveness (graph weight), result size, running time and  graph nodes, respectively. Here is the output of GSE + Refine for the above query example:

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
java -jar RSCS-main.jar test.ETest ${relationFilePath} ${similarityFilePath} ${graphFilePath} ${vertexTypeFilePath} ${queryRelation} ${AaType} ${queryID} ${l} ${queryK} ${tao}>test.txt 2>&1 &":

Method: GSE
AaType:346 l:3 queryK:10 tao:0.93
data read over
graph weight:0.9966554900568538
result size:3776
query time:5173.893169ms
graph nodes:[458758, 2490405, 4964431, 2277414, 7028846, 5857361, 2891812, 3424317, ....]
```
