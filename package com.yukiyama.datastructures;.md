**package** com.yukiyama.datastructures;



**import** java.util.ArrayList;

**import** java.util.Arrays;

**import** java.util.Collections;

**import** java.util.Comparator;

**import** java.util.HashMap;

**import** java.util.LinkedList;

**import** java.util.List;

**import** java.util.Map;

**import** java.util.PriorityQueue;

**import** java.util.Queue;



**public** **class** MyGraphDemo {



  **public** **static** **void** main(String[] args) {

​    MyGraphDemo demo = **new** MyGraphDemo();

​    System.***out\***.println("==== 实现9.2节 输出图的拓扑排序(Kahn算法)");

​    demo.demo1();

​    System.***out\***.println("==== 实现9.3.1节 输出图的无权最短路径");

​    demo.demo2();

​    System.***out\***.println("==== 实现9.3.2, 9.3.3节 输出图的赋权最短路径(Dijkstra/Bellman-Ford/SPFA算法)");

​    demo.demo3();

​    System.***out\***.println("==== 实现9.3.4节 输出无圈图的赋权最短路径(无圈图Dijkstra算法)");

​    demo.demo4();

​    System.***out\***.println("==== 实现9.3.5节 所有点对最短路径(Floyd算法)");

​    demo.demo5();

​    System.***out\***.println("==== 实现9.4节 计算两顶点间最大流(EK/Dinic算法)");

​    demo.demo6();

  }

   

  **public** **void** demo1() {

​    // 初始化graph step1，创建所有顶点实例

​    // graph: https://tva1.sinaimg.cn/large/008i3skNgy1gvaep9vlx5j60k80dowf502.jpg

​    Vertex<Integer> v1 = **new** Vertex<>(1); 

​    Vertex<Integer> v2 = **new** Vertex<>(2);

​    Vertex<Integer> v3 = **new** Vertex<>(3);

​    Vertex<Integer> v4 = **new** Vertex<>(4);

​    Vertex<Integer> v5 = **new** Vertex<>(5);

​    Vertex<Integer> v6 = **new** Vertex<>(6);

​    Vertex<Integer> v7 = **new** Vertex<>(7);

​    List<Vertex<Integer>> vertices = Arrays.*asList*(v1, v2, v3, v4, v5, v6 ,v7);

​    // 初始化邻接表

​    List<List<Vertex<Integer>>> adjLists = **new** ArrayList<>();

​    adjLists.add(Arrays.*asList*(v2, v3, v4));

​    adjLists.add(Arrays.*asList*(v4, v5));

​    adjLists.add(Arrays.*asList*(v6));

​    adjLists.add(Arrays.*asList*(v3, v6, v7));

​    adjLists.add(Arrays.*asList*(v4, v7));

​    adjLists.add(Arrays.*asList*());

​    adjLists.add(Arrays.*asList*(v6));

​    // 初始化graph step3，创建图实例

​    MyGraph<Integer> graph = **new** MyGraph<>(vertices, adjLists);

​    // 计算所有顶点入度

​    graph.indegree(); 

​    System.***out\***.print("图中各点入度为：");

​    graph.showIndegree();

​    // 输出“拓扑排序： [1, 2, 5, 4, 3, 7, 6]”

​    System.***out\***.println("拓扑排序： " + graph.topoSort().toString());

  }

   

  **public** **void** demo2() {

​    // 初始化graph2 step1，创建所有顶点实例

​    // graph2: https://tva1.sinaimg.cn/large/008i3skNgy1gvaeqec413j60ne0fqjs902.jpg

​    Vertex<Integer> v1 = **new** Vertex<>(1); 

​    Vertex<Integer> v2 = **new** Vertex<>(2);

​    Vertex<Integer> v3 = **new** Vertex<>(3);

​    Vertex<Integer> v4 = **new** Vertex<>(4);

​    Vertex<Integer> v5 = **new** Vertex<>(5);

​    Vertex<Integer> v6 = **new** Vertex<>(6);

​    Vertex<Integer> v7 = **new** Vertex<>(7);

​    List<Vertex<Integer>> vertices = Arrays.*asList*(v1, v2, v3, v4, v5, v6 ,v7);

​    // 

​    List<List<Vertex<Integer>>> adjLists = **new** ArrayList<>();

​    adjLists.add(Arrays.*asList*(v2, v4));

​    adjLists.add(Arrays.*asList*(v4, v5));

​    adjLists.add(Arrays.*asList*(v1, v6));

​    adjLists.add(Arrays.*asList*(v3, v5, v6, v7));

​    adjLists.add(Arrays.*asList*(v7));

​    adjLists.add(Arrays.*asList*());

​    adjLists.add(Arrays.*asList*(v6));

​    // 初始化graph2 step3，创建图实例

​    MyGraph<Integer> graph = **new** MyGraph<>(vertices, adjLists);

​    System.***out\***.print("v3为起点的无权最短路径：");

​    graph.showVertices(graph.shortestPathUnweighted(v3));

​    System.***out\***.print("v1到v7的最短路径: ");

​    graph.showVertices(graph.getPath(v1, v7));

  }

   

  **public** **void** demo3() {

​    // 初始化graph3 step1，创建所有顶点实例

​    // graph3: https://tva1.sinaimg.cn/large/008i3skNgy1gvaesammotj60m20fgq3s02.jpg

​    Vertex<Integer> v1 = **new** Vertex<>(1); 

​    Vertex<Integer> v2 = **new** Vertex<>(2);

​    Vertex<Integer> v3 = **new** Vertex<>(3);

​    Vertex<Integer> v4 = **new** Vertex<>(4);

​    Vertex<Integer> v5 = **new** Vertex<>(5);

​    Vertex<Integer> v6 = **new** Vertex<>(6);

​    Vertex<Integer> v7 = **new** Vertex<>(7);

​    List<Vertex<Integer>> vertices = Arrays.*asList*(v1, v2, v3, v4, v5, v6 ,v7);

​    //

​    List<List<Vertex<Integer>>> adjLists = **new** ArrayList<>();

​    adjLists.add(Arrays.*asList*(v2, v4));

​    adjLists.add(Arrays.*asList*(v4, v5));

​    adjLists.add(Arrays.*asList*(v1, v6));

​    adjLists.add(Arrays.*asList*(v3, v5, v6, v7));

​    adjLists.add(Arrays.*asList*(v7));

​    adjLists.add(Arrays.*asList*());

​    adjLists.add(Arrays.*asList*(v6));

​    // 初始化graph3 step2，设置每个顶点的邻接表和距离表

​    List<List<Integer>> adjValuesLists = **new** ArrayList<>();

​    adjValuesLists.add(Arrays.*asList*(2, 1));

​    adjValuesLists.add(Arrays.*asList*(3, 10));

​    adjValuesLists.add(Arrays.*asList*(4, 5));

​    adjValuesLists.add(Arrays.*asList*(2, 2, 8, 4));

​    adjValuesLists.add(Arrays.*asList*(6));

​    adjValuesLists.add(Arrays.*asList*());

​    adjValuesLists.add(Arrays.*asList*(1));

​    // 初始化graph3 step3，创建图实例

​    MyGraph<Integer> graph = **new** MyGraph<>(vertices, adjLists, adjValuesLists);

​    System.***out\***.print("赋权最短路径：");

​    // graph.showVertices(graph.shortestPathDijkstraBasic(v1)); // Dijkstra朴素版本

​    graph.showVertices(graph.shortestPathDijkstraPQ(v1)); // Dijkstra队列优化版本

​    // graph.showVertices(graph.shortestPathBF(v1)); // Bellman-Ford朴素版本

​    // graph.showVertices(graph.shortestPathSPFA(v1)); // Bellman-Ford的队列优化版本SPFA

​    System.***out\***.print("v1到v7的最短路径：");

​    graph.showVertices(graph.getPath(v1, v7));

​    System.***out\***.println("v1到v7的最短路径长：" + v7.getDistance()); // 直接给出

​    System.***out\***.println("v1到v7的最短路径长：" + graph.getPathValue(v1, v7)); // 通过递归寻找v7的前驱节点给出

  }

   

  **public** **void** demo4() {

​    // 初始化graph step1，创建所有顶点实例

​    // graph: https://tva1.sinaimg.cn/large/008i3skNgy1gvcj85fmasj60ha0cgt9q02.jpg

​    Vertex<Integer> v1 = **new** Vertex<>(1); 

​    Vertex<Integer> v2 = **new** Vertex<>(2);

​    Vertex<Integer> v3 = **new** Vertex<>(3);

​    Vertex<Integer> v4 = **new** Vertex<>(4);

​    Vertex<Integer> v5 = **new** Vertex<>(5);

​    Vertex<Integer> v6 = **new** Vertex<>(6);

​    Vertex<Integer> v7 = **new** Vertex<>(7);

​    List<Vertex<Integer>> vertices = Arrays.*asList*(v1, v2, v3, v4, v5, v6 ,v7);

​    //

​    List<List<Vertex<Integer>>> adjLists = **new** ArrayList<>();

​    adjLists.add(Arrays.*asList*(v2, v3, v4));

​    adjLists.add(Arrays.*asList*(v4, v5));

​    adjLists.add(Arrays.*asList*(v6));

​    adjLists.add(Arrays.*asList*(v3, v5, v6, v7));

​    adjLists.add(Arrays.*asList*(v7));

​    adjLists.add(Arrays.*asList*());

​    adjLists.add(Arrays.*asList*(v6));

​    // 初始化graph3 step2，设置每个顶点的邻接表和距离表

​    List<List<Integer>> adjValuesLists = **new** ArrayList<>();

​    adjValuesLists.add(Arrays.*asList*(2, 4, 1));

​    adjValuesLists.add(Arrays.*asList*(3, 10)); // 将10改成-10可以测试有负边值的图

​    adjValuesLists.add(Arrays.*asList*(5));

​    adjValuesLists.add(Arrays.*asList*(2, 2, 8, 4));

​    adjValuesLists.add(Arrays.*asList*(6));

​    adjValuesLists.add(Arrays.*asList*());

​    adjValuesLists.add(Arrays.*asList*(1));

​    // 初始化graph3 step3，创建图实例

​    MyGraph<Integer> graph = **new** MyGraph<>(vertices, adjLists, adjValuesLists);

​    // 计算所有顶点入度

​    graph.indegree(); 

​    System.***out\***.print("DijkstraDAG赋权最短路径：");

​    graph.showVertices(graph.shortestPathDijkstraDAG(v1));

​    System.***out\***.print("v1到v7的最短路径：");

​    graph.showVertices(graph.getPath(v1, v7));

​    System.***out\***.println("v1到v7的最短路径长：" + graph.getPathValue(v1, v7));

  }

   

  **public** **void** demo5() {

​    // 初始化graph https://tva1.sinaimg.cn/large/008i3skNgy1gvcj85fmasj60ha0cgt9q02.jpg

​    // 1.创建所有顶点实例，创建顶点表

​    Vertex<Integer> v1 = **new** Vertex<>(1); 

​    Vertex<Integer> v2 = **new** Vertex<>(2);

​    Vertex<Integer> v3 = **new** Vertex<>(3);

​    Vertex<Integer> v4 = **new** Vertex<>(4);

​    Vertex<Integer> v5 = **new** Vertex<>(5);

​    Vertex<Integer> v6 = **new** Vertex<>(6);

​    Vertex<Integer> v7 = **new** Vertex<>(7);

​    List<Vertex<Integer>> vertices = Arrays.*asList*(v1, v2, v3, v4, v5, v6 ,v7);

​    // 2.设置顶点邻接关系，创建邻接表

​    List<List<Vertex<Integer>>> adjLists = **new** ArrayList<>();

​    adjLists.add(Arrays.*asList*(v2, v3, v4));

​    adjLists.add(Arrays.*asList*(v4, v5));

​    adjLists.add(Arrays.*asList*(v6));

​    adjLists.add(Arrays.*asList*(v3, v5, v6, v7));

​    adjLists.add(Arrays.*asList*(v7));

​    adjLists.add(Arrays.*asList*());

​    adjLists.add(Arrays.*asList*(v6));

​    // 3.设置顶点间距离(边权)，创建距离表(边权表)

​    List<List<Integer>> adjValuesLists = **new** ArrayList<>();

​    adjValuesLists.add(Arrays.*asList*(2, 4, 1));

​    adjValuesLists.add(Arrays.*asList*(3, 10)); // 将10改成-10可以测试有负边值的图

​    adjValuesLists.add(Arrays.*asList*(5));

​    adjValuesLists.add(Arrays.*asList*(2, 2, 8, 4));

​    adjValuesLists.add(Arrays.*asList*(6));

​    adjValuesLists.add(Arrays.*asList*());

​    adjValuesLists.add(Arrays.*asList*(1));

​    // 4.创建图实例

​    MyGraph<Integer> graph = **new** MyGraph<>(vertices, adjLists, adjValuesLists);

​    System.***out\***.println("计算所有点对赋权最短路径");

​    graph.allShortestPathFloyd();

​    System.***out\***.print("v1到v7的最短路径：");

​    graph.showVertices(graph.getPath(v1, v7));

​    System.***out\***.println("v1到v7的最短路径长：" + graph.getPathValue(v1, v7));

  }

   

  **public** **void** demo6() {

​    // 初始化graph https://tva1.sinaimg.cn/large/008i3skNgy1gvfw5jat8ij608a0aiaa502.jpg

​    // 1.创建所有顶点实例，创建顶点表

​    Vertex<String> s = **new** Vertex<>("s"); 

​    Vertex<String> a = **new** Vertex<>("a");

​    Vertex<String> b = **new** Vertex<>("b");

​    Vertex<String> c = **new** Vertex<>("c");

​    Vertex<String> d = **new** Vertex<>("d");

​    Vertex<String> e = **new** Vertex<>("e");

​    Vertex<String> f = **new** Vertex<>("f");

​    Vertex<String> g = **new** Vertex<>("g");

​    Vertex<String> h = **new** Vertex<>("h");

​    Vertex<String> i = **new** Vertex<>("i");

​    Vertex<String> j = **new** Vertex<>("j");

​    Vertex<String> t = **new** Vertex<>("t");

​    List<Vertex<String>> vertices = Arrays.*asList*(s, a, b, c, d, e, f, g, h, i, j, t);

​    // 2.设置顶点邻接关系，创建邻接表

​    List<List<Vertex<String>>> adjLists = **new** ArrayList<>();

​    adjLists.add(Arrays.*asList*(e, a, b)); // adjList for s

​    adjLists.add(Arrays.*asList*(b, c, d)); // adjList for a

​    adjLists.add(Arrays.*asList*(d)); // adjList for b

​    adjLists.add(Arrays.*asList*(t)); // adjList for c

​    adjLists.add(Arrays.*asList*(t)); // adjList for d

​    adjLists.add(Arrays.*asList*(f)); // adjList for e

​    adjLists.add(Arrays.*asList*(g)); // adjList for f

​    adjLists.add(Arrays.*asList*(h)); // adjList for g

​    adjLists.add(Arrays.*asList*(i)); // adjList for h

​    adjLists.add(Arrays.*asList*(j)); // adjList for i

​    adjLists.add(Arrays.*asList*(t)); // adjList for j

​    adjLists.add(Arrays.*asList*()); // adjList for t

​    // 3.设置顶点间距离(边权)，创建距离表(边权表)

​    List<List<Integer>> adjValuesLists = **new** ArrayList<>();

​    adjValuesLists.add(Arrays.*asList*(10, 3, 2)); // s

​    adjValuesLists.add(Arrays.*asList*(1, 3, 4)); // a 将10改成-10可以测试有负边值的图

​    adjValuesLists.add(Arrays.*asList*(2)); // b

​    adjValuesLists.add(Arrays.*asList*(2)); // c

​    adjValuesLists.add(Arrays.*asList*(3)); // d

​    adjValuesLists.add(Arrays.*asList*(10)); // e

​    adjValuesLists.add(Arrays.*asList*(10)); // f

​    adjValuesLists.add(Arrays.*asList*(10)); // g

​    adjValuesLists.add(Arrays.*asList*(10)); // h

​    adjValuesLists.add(Arrays.*asList*(10)); // i

​    adjValuesLists.add(Arrays.*asList*(1)); // j

​    adjValuesLists.add(Arrays.*asList*());

​     

​    // graph: https://tva1.sinaimg.cn/large/008i3skNgy1gvfw5jat8ij608a0aiaa502.jpg

//    Vertex<String> s = new Vertex<>("s"); 

//    Vertex<String> a = new Vertex<>("a");

//    Vertex<String> b = new Vertex<>("b");

//    Vertex<String> c = new Vertex<>("c");

//    Vertex<String> d = new Vertex<>("d");

//    Vertex<String> t = new Vertex<>("t");

//    List<Vertex<String>> vertices = Arrays.asList(s, a, b, c, d, t);

//     

//    List<List<Vertex<String>>> adjLists = new ArrayList<>();

//    adjLists.add(Arrays.asList(a, b)); // adjList for s

//    adjLists.add(Arrays.asList(b, c, d)); // adjList for a

//    adjLists.add(Arrays.asList(d)); // adjList for b

//    adjLists.add(Arrays.asList(t)); // adjList for c

//    adjLists.add(Arrays.asList(t)); // adjList for d

//    adjLists.add(Arrays.asList()); // adjList for t

//     

//    List<List<Integer>> adjValuesLists = new ArrayList<>();

//    adjValuesLists.add(Arrays.asList(3, 2)); // s

//    adjValuesLists.add(Arrays.asList(1, 3, 4)); // a 将10改成-10可以测试有负边值的图

//    adjValuesLists.add(Arrays.asList(2)); // b

//    adjValuesLists.add(Arrays.asList(2)); // c

//    adjValuesLists.add(Arrays.asList(3)); // d

//    adjValuesLists.add(Arrays.asList());

​     

​    // 4.创建图实例

​    MyGraph<String> graph = **new** MyGraph<>(vertices, adjLists, adjValuesLists);

​     

​    System.***out\***.println("s到t的最大流：" + graph.maxFlowEK(s, t)); // 朴素Dinic

​    // System.out.println("s到t的最大流：" + graph.maxFlowDinicBasic(s, t)); // 朴素Dinic

​    // System.out.println("s到t的最大流：" + graph.maxFlowDinic(s, t)); // 多路增广Dinic

  }



}



/**

 \* 定义图类为泛型是因为顶点类定义为泛型

 */

**class** MyGraph<E>{

   

  **private** List<Vertex<E>> vertices;

  **private** Map<Vertex<E>, List<Vertex<E>>> adjListsUnweighted;

  **private** Map<Vertex<E>, Map<Vertex<E>, Integer>> adjListsWeighted;

  // private Map<Vertex<E>, Integer> remainFlow;

  **private** **final** **static** **int** ***NO_EDGE_VALUE\*** = 0;

  **private** **final** **static** **int** ***AUGMENTING_PATH_NOT_FOUND\*** = -1;

   

  **public** MyGraph (List<Vertex<E>> vertices) {

​    **this**.vertices = vertices;

  }

  // 无权图

  **public** MyGraph (List<Vertex<E>> vertices, List<List<Vertex<E>>> adjLists) {

​    **this**.vertices = vertices;

​    adjListsUnweighted = **new** HashMap<>();

​    **for** (**int** i = 0; i < vertices.size(); i++) {

​      adjListsUnweighted.put(vertices.get(i), adjLists.get(i));

​    }

  }

  // 有权图

  **public** MyGraph (List<Vertex<E>> vertices, List<List<Vertex<E>>> adjLists, List<List<Integer>> adjValuesLists) {

​    **this**(vertices, adjLists);

​    initAdjListsWeighted();

​    **for** (**int** i = 0; i < vertices.size(); i++) {

​      List<Vertex<E>> adjVertices = adjLists.get(i);

​      List<Integer> adjValues = adjValuesLists.get(i);

​      **for** (**int** j = 0; j < adjVertices.size(); j++) {

​        adjListsWeighted.get(vertices.get(i)).put(adjVertices.get(j), adjValues.get(j)) ;

​      }

​    }

  }

   

  /**

   \* 初始化有权图邻接哈希表 Map<Vertex<E>, Map<Vertex<E>, Integer>>

   \* 结构为 Map<key1, value1 = Map<key2, value2>>，key1表示图中的顶点，key1对应的value1是哈希表，

   \* 为key1顶点对应的邻接顶点信息，key2是key1的邻接顶点，value2是(key1, key2)边的权值

   */

  **private** **void** initAdjListsWeighted() {

​    adjListsWeighted = **new** HashMap<>();

​    **for**(Vertex<E> v : vertices) {

​      Map<Vertex<E>, Integer> adjValuesMap = **new** HashMap<>(); 

​      **for** (Vertex<E> w : vertices) {

​        adjValuesMap.put(w, ***NO_EDGE_VALUE\***);

​      }

​      adjListsWeighted.put(v,adjValuesMap);

​    }

  }

  **public** **void** clearVertices() {

​    **for** (Vertex<E> v : vertices) {

​      v.clear();

​    }

  }

  **public** List<Vertex<E>> getAdjList(Vertex<E> v){

​    **return** adjListsUnweighted.get(v);

  }

  **public** **void** setAdjList(Vertex<E> v, List<Vertex<E>> adjList){

​    adjListsUnweighted.put(v, adjList);

  }

  **public** **int** setEdgeValue(Vertex<E> v, Vertex<E> w, **int** value) {

​    **return** adjListsWeighted.get(v).put(w, value);

  }

  **public** **int** getEdgeValue(Vertex<E> v, Vertex<E> w) {

​    **return** adjListsWeighted.get(v).get(w);

  }

   

  /**

   \* 计算入度，时间复杂度为 O(|E|+|V|)

   */

  **public** **void** indegree() {

​    **for** (Vertex<E> v : vertices) {

​      List<Vertex<E>> adjList = getAdjList(v);

​      **for** (Vertex<E> w : adjList) {

​        **if**(w != **null**) {

​          w.setIndegree(w.getIndegree() + 1);

​        }

​      }

​    }

  }

   

  /**

   \* 打印所有顶点的入度信息

   */

  **public** **void** showIndegree() {

​    **for** (Vertex<E> v : vertices) {

​      System.***out\***.printf("%s: %d; ", v.getName(), v.getIndegree());

​    }

​    System.***out\***.println();

  }

   

  /**

   \* 输出拓扑排序，时间复杂度 O(|E|+|V|)

   */

  **public** List<E> topoSort() {

​    List<E> res = **new** ArrayList<>();

​    Queue<Vertex<E>> q = **new** LinkedList<>();

​    **int** counter = 0;

​     

​    // 遍历顶点，将入度为0的顶点入队

​    **for** (Vertex<E> v : vertices) {

​      **if**(v.getIndegree() == 0) {

​        q.add(v);

​      }

​    }

​    // 若队列不空，队首顶点v出队，输出到res，counter加1(用于设置拓扑排序序号和后续判断是否有圈)，

​    // 设置v的拓扑排序序号，接着将v的邻接顶点w的入度减1，同时检w的入度是否变为0，是则入队

​    **while**(!q.isEmpty()) {

​      Vertex<E> v = q.remove();

​      res.add(v.getName());

​      counter++;

​      v.setTopoNum(counter);

​      **for** (Vertex<E> w : getAdjList(v)) {

​        w.setIndegree(w.getIndegree() - 1);

​        **if**(w.getIndegree() == 0) {

​          q.add(w);

​        }

​      }

​    }

​    // 若while后counter不等于顶点数，则说明该有向图有圈

​    **if**(counter != vertices.size()) {

​      System.***err\***.println("Cycle found!");

​    }

​     

​    **return** res;

  }

   

  /**

   \* 单源无权最短路径方法，时间复杂度 O(|E|+|V|)

   \* 与拓扑排序时间复杂度类似

   */

  **public** List<Vertex<E>> shortestPathUnweighted(Vertex<E> s){

​    List<Vertex<E>> res = **new** ArrayList<>();

​    Queue<Vertex<E>> q = **new** LinkedList<>();

​    s.setDistance(0); // 将起始顶点的距离设为0

​    q.add(s); // 起始顶点入队

​     

​    **while**(!q.isEmpty()) {

​      Vertex<E> v = q.remove();

​      res.add(v);

​      // 每次顶点v出队，考察其邻接顶点的距离

​      **for** (Vertex<E> w : getAdjList(v)) {

​        **if**(w.getDistance() == Vertex.***DEFAULT_DIST\***) { // 若为默认值，说明该顶点未访问过

​          w.setDistance(v.getDistance() + 1); // 将其距离置为v.getDistance() + 1

​          w.setPre(v); // 将v设置为w的其前驱顶点

​          q.add(w); // w入队

​        }

​      }

​    }

​     

​    **return** res;

  }  

   

  /**

   \* 单源无权最短路径方法，时间复杂度 O(|V|^2)

   \* 缺点在于前两个for导致了O(|V|^2)的时间复杂度，外层循环没有利用程序运行后期多数顶点均已known的信息

   */

  **public** List<Vertex<E>> shortestPathUnweightedSlow(Vertex<E> s){

​    List<Vertex<E>> res = **new** ArrayList<>();

​    s.setDistance(0);

​    **for** (**int** currentDist = 0; currentDist < vertices.size(); currentDist++) {

​      **for** (Vertex<E> v : vertices) {

​        **if**(!v.isKnown() && v.getDistance() == currentDist) {

​          v.setKnown(**true**);

​          res.add(v);

​          **for** (Vertex<E> w : getAdjList(v)) {

​            **if**(w.getDistance() == -1) {

​              w.setDistance(currentDist + 1);

​              w.setPre(v);

​            }

​          }

​        }

​      }

​       

​    }

​    **return** res;

  }  

   

  /**

   \* Dijkstra算法朴素版本

   \* 单源赋权最短路径方法，时间复杂度 O(|E|+|V|^2)

   \* 算法过程：

   \* 1. 初始化。

   \* 将所有顶点到源点的距离(后续简称距离)设置为无距离(如无穷大)，本程序设置为-1.

   \* 所有顶点的“距离是否已最终确定”属性布尔值isKnown设置为false。

   \* 2. 在未确定距离的顶点中找到距离最小者v，将其置为距离已确定，即isKnown=true。

   \* 3. 对v的未确定距离邻接顶点进行松弛操作。

   \* 松弛操作：

   \* 对v的邻接顶点w考察是否有 dv + d(v, w) < dw，是则用<号左边的结果更新dw。

   \* 

   \* 时间复杂度分析：

   \* 1. 寻找拥有到源点最小距离的顶点的时间,O(|V|^2)

   \* 每次寻找拥有到源点最小距离的顶点花费O(|V|^2)，这是因为

   \* 总是先遍历一遍顶点确认存在尚未访问的顶点O(|V|)，

   \* 再遍历一遍顶点寻找未确定距离的顶点中距离源点最短的顶点,O(|V|)。

   \* 2. 所有顶点可能被更新的次数上限，O(|E|)

   \* 考虑顶点w到源点距离dw被更新的次数，与w的入边有关，因为无论是对w的初始化还是

   \* 更新，都是由其前驱(入边)来决定的，所有顶点的入边数等于总边数，故为O(|E|)。

   \* 于是总的时间复杂度是 O(|E|+|V|^2)。

   \* 注1：注释中说的“距离“均指到源点s的距离

   \* 注2：本实现对有负值边的图无效

   \* 

   \* 为了更清楚地分析时间复杂度，将检查未确定距离顶点的方法checkUnknown和

   \* 获取未确定距离顶点中最小者的方法getUnknownMin写在外部。

   */

  **public** List<Vertex<E>> shortestPathDijkstraBasic(Vertex<E> s){

​    List<Vertex<E>> res = **new** ArrayList<>(); 

​    s.setDistance(0);

​     

​    // 遍历所有顶点确认是否有未访问过的顶点， O(|V|)

​    **while**(checkUnknown(vertices)) { 

​      // 从unknowns中取出距离最小的顶点， O(|V|)

​      Vertex<E> v = getUnknownMin(vertices);

​      v.setKnown(**true**); // 设置为已访问

​      res.add(v); // 按顺序输出已确定距离的顶点

​      // 松弛v的邻接顶点w，O(|E|)

​      **for** (Vertex<E> w : getAdjList(v)) {

​        **if**(!w.isKnown() && getEdgeValue(v, w) > 0) { // 松弛对象是未确定距离的w

​          **int** dw = v.getDistance() + getEdgeValue(v, w);

​          **if**(w.getDistance() == -1 || dw < w.getDistance()) { // 松弛条件

​            w.setDistance(dw);

​            w.setPre(v);

​          }

​        }

​      }

​    }

​     

​    **return** res;

  }

   

  /**

   \* 检查顶点列表中是否存在unknown顶点

   */

  **private** **boolean** checkUnknown(List<Vertex<E>> vertices) {

​    **for** (Vertex<E> v : vertices) {

​      **if**(!v.isKnown()) {

​        **return** **true**;

​      }

​    }

​    **return** **false**;

  }

   

  /**

   \* 返回顶点列表中unknown且到源点距离最小的顶点

   */

  **private** Vertex<E> getUnknownMin(List<Vertex<E>> list){

​    Vertex<E> min = **new** Vertex<>();

​    // 因为本程序距离默认值为-1，所以先将初始min的距离设置为无穷大

​    min.setDistance(Integer.***MAX_VALUE\***); 

​    **for** (**int** i = 0; i < vertices.size(); i++) {

​      Vertex<E> cur = vertices.get(i);

​      // 在unknown顶点中找到有距离的(非负)距离最小者

​      **if**(!cur.isKnown() && cur.getDistance() != -1 && cur.getDistance() < min.getDistance()) {

​        min = cur;

​      }

​    }

​    **return** min;

  }

   

  /**

   \* Dijkstra算法优先队列版本

   \* 针对算法过程中的第2点，利用一个优先队列pq存储顶点

   \* 由于从pq判空耗时O(|V|)获取距离最小者耗时O(log|V|),

   \* 所以总的时间复杂度是O(|E|+|V|log|V|)。

   \* 注意：优先队列判空操作isEmpty()操作本身是常数时间操作，

   \* 但总共要执行|V|次.

   */

  **public** List<Vertex<E>> shortestPathDijkstraPQ(Vertex<E> s){

​    List<Vertex<E>> res = **new** ArrayList<>();

​    s.setDistance(0);

​    // 泛型PriorityQueue的写法，重写compare方法，比较元素getDistance()方法的结果，小顶堆

​    PriorityQueue<Vertex<E>> pq = **new** PriorityQueue<>(**new** Comparator<Vertex<E>>() {

​      @Override

​      **public** **int** compare(Vertex<E> v1, Vertex<E> v2) {

​        **return** v1.getDistance() - v2.getDistance();

​      }

​    });

​    pq.add(s);

​     

​    **while**(!pq.isEmpty()) {

​      // pq取出并删除堆顶，时间复杂度为O(log|V|)

​      // 小顶堆pq的删除操作同时保证了两点

​      // 1.堆中的顶点一定都是未确定距离的 2. 返回的v一定是未确定距离顶点里距离最小的

​      Vertex<E> v = pq.remove(); 

​      res.add(v);

​      **for** (Vertex<E> w : getAdjList(v)) {

​        **int** vwEdgeValue = getEdgeValue(v, w);

​        **int** svwDist = v.getDistance() + vwEdgeValue;

​        // 如果w为未更新过距离的顶点或源点s经过v到w的距离svwDist比当前w的距离更短，

​        // 则更新其距离为svwDist。

​        **if**(w.getDistance() == -1 || svwDist < w.getDistance()) { 

​          // 将初次赋有效值的顶点w加入到pq中，以保证算法过程第2步总是

​          // 能够在所有“具有有效距离值但未最终确定距离”中寻找距离最小者

​          **if**(w.getDistance() == -1) { 

​            pq.add(w);

​          }

​          w.setDistance(svwDist); // 更新dw

​          w.setPre(v); // 将v设置为w的前驱

​        }

​      }

​    }

​     

​    **return** res;

  }

   

  /**

   \* SPFA算法

   \* https://en.wikipedia.org/wiki/Shortest_Path_Faster_Algorithm

   \* 本质上是Bellman-Ford算法的一种应用，与Bellman-Ford一样，对有负边的图同样适用。

   \* 1994年西南交通大学的段凡丁

   \* 但注意该算法不能称作Dijkstra算法，

   \* 单源赋权最短路径方法，时间复杂度 O()

   \* 注1：注释中说的“距离“均指到源点s的距离

   \* 注2：本实现对有负值边的图同样有效，因为只会将未访问过的顶点入堆

   */

  **public** List<Vertex<E>> shortestPathSPFA(Vertex<E> s){

​    List<Vertex<E>> res = **new** ArrayList<>();

​    s.setDistance(0);

​    // 泛型PriorityQueue的写法，重写compare方法，比较元素getDistance()方法的结果，小顶堆

​    Queue<Vertex<E>> q = **new** LinkedList<>();

​    q.add(s);

​    **while**(!q.isEmpty()) {

​      Vertex<E> v = q.remove(); // 二叉堆取除并删除堆顶，时间复杂度为O(log|V|)

​      res.add(v);

​      **for** (Vertex<E> w : getAdjList(v)) {

​        **int** vwEdgeValue = getEdgeValue(v, w);

​        **int** wDist = v.getDistance() + vwEdgeValue;

​        // 如果w未被访问过，更新其距离为“v的距离+vw边权”

​        **if**(w.getDistance() == -1) { 

​          w.setDistance(wDist);

​          w.setPre(v); // 将v设置为w的前驱

​          q.add(w); // 将w加入优先队列，则v的所有邻接顶点w会以小顶堆序存放于pq中

​        }

​        // 若w已被访问过，但源点通过v到w的距离小于源点到w的距离，更新源点到w的距离

​        **else** **if**(wDist < w.getDistance()) { // 若源点经过顶点v到顶点w的距离小于源点到w的距离，更新源点w的距离

​          w.setDistance(wDist);

​          w.setPre(v);

​        }

​      }

​    }

​     

​    **return** res;

  }

   

  /**

   \* 无圈图Dijkstra算法

   \* 不同于通用Dijkstra算法中以堆保存节点的方式，采用队列保存顶点并以拓扑排序方式输出，

   \* 只有入度为0的顶点会被选取入队，此时它的距离不会再被改变(入度0，无入边)，也就是它的距离

   \* 在此时就最终确定了，所以该算法是可行的，距离更新花费常数时间，故复杂度与拓扑排序一致。

   \* 时间复杂度 O(|E|+|V|)

   \* 注1：注释中说的“距离“均指到源点s的距离

   \* 注2：本实现对有负值边的图同样有效，因为只会将未访问过的顶点入队

   */

  **public** List<Vertex<E>> shortestPathDijkstraDAG(Vertex<E> s){

​    List<Vertex<E>> res = **new** ArrayList<>();

​    Queue<Vertex<E>> q = **new** LinkedList<>();

​    s.setDistance(0);

​    q.add(s);

​    **while**(!q.isEmpty()) {

​      Vertex<E> v = q.remove(); 

​      v.setKnown(**true**);

​      res.add(v);

​      **for** (Vertex<E> w : getAdjList(v)) {

​        **int** vwCost = getEdgeValue(v, w); // 获取边(v,w)的权值

​        **int** v_vmCost = v.getDistance() + vwCost; // 顶点v的距离值+边(v,w)的权值

​        // 如果w未被访问过，更新其距离为“v的距离+vw边权”

​        **if**(w.getDistance() == Vertex.***DEFAULT_DIST\***) { 

​          w.setDistance(v_vmCost);

​          w.setPre(v);

​        }

​        // 若w已被访问过，但通过v到w所得到的距离小于w的距离

​        **else** **if**(v_vmCost < w.getDistance()) { // 若“v的距离+vw边权”小于w的距离，更新w的距离

​          w.setDistance(v_vmCost);

​          w.setPre(v);

​        }

​        w.setIndegree(w.getIndegree() - 1); // w的入度减1

​        **if**(w.getIndegree() == 0) { // 考察w此时入度是否为0，为0则入队

​          q.add(w);

​        }

​      }

​    }

​     

​    **return** res;

  }

   

//  public List<Vertex<E>> allShortestPathBF(Vertex<E> s){

//    List<Vertex<E>> res = new ArrayList<>();

//    for(Vertex<E> v : vertices) {

//      for(Vertex<E> w : vertices) {

//        int svwDist = v.getDistance() + getEdgeValue(v, w);

//        if(svwDist < w.getDistance()) {

//           

//        }

//      }

//    }

//  }

   

  /**

   \* 无圈图所有顶点对的最短路径算法

   \* 从v到w，将k视作中间顶点，dist(v,k) + dist(k, w) < dist(v, w)时

   \* 更新w的距离。动态规划思想，复杂度O(|V|^3)。

   */

  **public** **void** allShortestPathFloyd(){

​    **for** (Vertex<E> v : vertices) {

​      **for** (Vertex<E> k : vertices) {

​        **for** (Vertex<E> w : vertices) {

​          // 若v>k,k>w,v>w三者均存在时，考察dist(v,k) + dist(k, w) < dist(v, w)

​          **if**(getEdgeValue(v, k) > 0 && getEdgeValue(v, w) > 0 && getEdgeValue(k, w) > 0) {

​            **int** vk = getEdgeValue(v, k);

​            **int** kw = getEdgeValue(k, w);

​            **int** vw = getEdgeValue(v, w);

​            **if**(vk + kw < vw){

​              w.setDistance(vk + kw); 

​              w.setPre(k);

​            }

​            **else** {

​              w.setDistance(vw);

​              w.setPre(v);

​            }

​          }

​        }

​      }

​    }

  }

   

  /**

   \* 求顶点src到顶点des的最大流

   \* Edmonds-Karp算法

   */

  **public** **int** maxFlowEK(Vertex<E> src, Vertex<E> des) {

​    **int** incFlow = 0;

​    **int** maxFlow = 0;

​    // 不断寻找增广路径直到找不到 (== -1时)

​    **while**((incFlow = augPathBFS(src, des)) != ***AUGMENTING_PATH_NOT_FOUND\***) {

​      Vertex<E> current = des;

​      // 每找到一条增广路径，从des顶点开始，向前对路径上每一条边(src > des方向)

​      // 执行+inc操作，对其反向边执行-inc操作，直到到达src顶点为止

​      **while**(current != src) {

​        Vertex<E> pre = current.getPre();

​        setEdgeValue(pre, current, getEdgeValue(pre, current) - incFlow); // 路径上的边 -inc

​        setEdgeValue(current, pre, getEdgeValue(current, pre) + incFlow); // 路径上的反向边 +inc

​        current = pre;

​      }

​      clearVertices(); // 清除顶点访问标志

​      maxFlow += incFlow; // 将每次增广路带来的流量增加到maxFlow上

​    }

​     

​    **return** maxFlow;

  }

   

  /**

   \* bfs方式寻找增广路径，返回增广路径中最小权边的权值

   */

  **private** **int** augPathBFS(Vertex<E> src, Vertex<E> des){

​    **int** res = Integer.***MAX_VALUE\***;

​    Queue<Vertex<E>> q = **new** LinkedList<>();

​    q.add(src); // 起始顶点入队

​    **boolean** isExist = **false**;

​     

​    **while**(!q.isEmpty()) {

​      Vertex<E> v = q.remove();

​      **if**(v == des) {

​        isExist = **true**;

​        **break**;

​      }

​      // 每次顶点v出队，考察其邻接顶点的距离

​      **for** (Vertex<E> w : getAdjList(v)) {

​        // 寻找未访问过且存在邻接顶点 (以是否有前驱节点作为是否访问过的标志，效果相当于!w.isKnown())

​        **if**(w.getPre() == **null** && getEdgeValue(v, w) > 0) {

​          w.setPre(v);

​          q.add(w); // w入队

​          res = Math.*min*(res, getEdgeValue(v, w)); // 更新增广路最小边权值

​        }

​      }

​    }

​    **if**(!isExist) {

​      **return** ***AUGMENTING_PATH_NOT_FOUND\***;

​    }

​     

​    **return** res;

  }

   

  /**

   \* 求顶点src到顶点des的最大流

   \* Dinic算法朴素版

   \* 朴素点在于在当前分层图中以DFS以最短路径找到增广路后，会再从src出发按同样的方法寻找下一条

   */

  **public** **int** maxFlowDinicBasic(Vertex<E> src, Vertex<E> des) {

​    **int** maxFlow = 0;

​    // 不断构建分层图

​    **while**(levelGraphBFS(src, des)) {

​      // 在一个确定有增广路的分层图中，求多条增广路可发送流的和，加入到最大流中

​      maxFlow += singleAugPathDfsDrive(src, des);

​      // 当前分层图处理完毕后重置顶点的层号信息

​      clearVertices();

​    }

​     

​    **return** maxFlow;

  }

   

  /**

   \* bfs方式构建分层图，返回判断是否存在增广路径的布尔值

   */

  **private** **boolean** levelGraphBFS(Vertex<E> src, Vertex<E> des){

​    Queue<Vertex<E>> q = **new** LinkedList<>();

​    src.setLevel(0);

​    q.add(src); // 起始顶点入队

​    **boolean** isExist = **false**; // 包含des的分层图是否存在，即当前图是否存在增广路

​     

​    **while**(!q.isEmpty()) {

​      **int** levelSize = q.size(); // 获取当前队列大小，也即本层顶点个数

​      **for** (**int** i = 0; i < levelSize; i++) { // 分配层号的关键

​        Vertex<E> v = q.remove(); // 由于有levelSize的边界控制，本轮for只会将本层顶点出队

​        // 每次顶点v出队

​        **for** (Vertex<E> w : getAdjList(v)) {

​          // 寻找未访问过且存在的邻接顶点，通过level标志判断是否访问过，所以不需要!isKnown条件

​          **if**(w.getLevel() == -1 && getEdgeValue(v, w) > 0) {

​            w.setLevel(v.getLevel() + 1);

​            q.add(w); // w入队

​          }

​        }

​      }

​    }

​    **if**(des.getLevel() != -1) {

​      **return** isExist = **true**;

​    }

​     

​    **return** isExist;

  }

   

  /**

   \* 求当前分层图下所有增广路径发送流的和

   */

  **private** **int** singleAugPathDfsDrive(Vertex<E> src, Vertex<E> des) {

​    **int** singleAugFlow = Integer.***MAX_VALUE\***;

​    **int** multipleAugFlow = 0;

​    // 将singleAugPathDfs方法换成singleAugPathDfsCurrentEdgeOpt应用当前弧优化

​    **while**((singleAugFlow = singleAugPathDfsCurrentEdgeOpt(des, src, singleAugFlow)) > 0) {

​    // while((singleAugFlow = singleAugPathDfs(des, src, singleAugFlow)) > 0) {

​      multipleAugFlow += singleAugFlow;

​    }

​    **return** multipleAugFlow;

  }

   

  /*

   \* 未经优化的朴素Dinic算法

  private int singleAugPathDfs(Vertex<E> des, Vertex<E> current, int singleAugFlow){

​    if(current == des) {

​      return singleAugFlow;

​    }

​    for(Vertex<E> w : getAdjList(current)) {

​      // 层号关系是关键 w.getLevel() == current.getLevel() + 1

​      // 保证了每次dfs总是当前图src到des的最短路径

​      if(w.getLevel() == current.getLevel() + 1 && getEdgeValue(current, w) > 0) {

​        int minEdge = singleAugPathDfs(des, w, Math.min(singleAugFlow, getEdgeValue(current, w)));

​        if(minEdge > 0) {

​          setEdgeValue(current, w, getEdgeValue(current, w) - minEdge);

​          setEdgeValue(w, current, getEdgeValue(w, current) + minEdge);

​          return minEdge; 

​        }

​      }

​    }

​     

​    return 0;

  }

  */

   

  /**

   \* 朴素Dinic算法当前弧优化版本

   \* 以DFS方式在当前分层图上执行一次增广

   \* 返回此条增广路可发送流

   */

  **private** **int** singleAugPathDfsCurrentEdgeOpt(Vertex<E> des, Vertex<E> current, **int** singleAugFlow){

​    **if**(current == des) {

​      **return** singleAugFlow;

​    }

​    **boolean** blockFlag = **true**;

​    **for**(Vertex<E> w : getAdjList(current)) {

​      // 层号关系是关键 w.getLevel() == current.getLevel() + 1

​      // 保证了每次dfs总是当前图src到des的最短路径

​      // 添加 !w.isBlocked() 条件实现当前弧优化

​      **if**(!w.isBlocked() && w.getLevel() == current.getLevel() + 1 && getEdgeValue(current, w) > 0) {

​        blockFlag = **false**;

​        **int** minEdge = singleAugPathDfsCurrentEdgeOpt(des, w, Math.*min*(singleAugFlow, getEdgeValue(current, w)));

​        **if**(minEdge > 0) {

​          setEdgeValue(current, w, getEdgeValue(current, w) - minEdge);

​          setEdgeValue(w, current, getEdgeValue(w, current) + minEdge);

​          **return** minEdge; 

​        }

​      }

​    }

​    // 若current经过整个for都无法找到满足条件的邻接顶点，即blockFlag保持不变，则将current置为阻塞

​    **if**(blockFlag) {

​      current.setBlock(**true**);

​    }

​     

​    **return** 0;

  }

   

  /**

   \* 求顶点src到顶点des的最大流

   \* Dinic算法多路增广优化版

   \* 在朴素版本中，每次主调用DFS找到一条增广路后会一路后退直到本次主调结束。

   \* 本优化版通过对每个顶点维护可用流flow，找到增广路后在回退过程中寻找其他增广路，

   \* 实现对一次主调用DFS完成当前分层图下所有增广路的搜索。

   */

  **public** **int** maxFlowDinic(Vertex<E> src, Vertex<E> des) {

​    **int** maxFlow = 0;

​    // 不断构建分层图，在一个确定有增广路的分层图中，求多条增广路可发送流的和，加入到最大流中

​    **while**(levelGraphBFS(src, des)) {

​      // 每次在当前分层图下多路增广前设置src的可用流

​      src.setRemainFlow(Integer.***MAX_VALUE\***); 

​      maxFlow += multipleAugPathDfs(src, des);

​      clearVertices(); // 每次结束当前分层图下的多路增广后重置顶点信息(层号信息)

​    }

​     

​    **return** maxFlow;

  }

   

  /**

   \* 以DFS方式查找当前分层图上所有增广路

   \* 返回这些增广路可发送流之和

   */

  **private** **int** multipleAugPathDfs(Vertex<E> current, Vertex<E> des) {

​    **int** incFlow = 0;

​    // 如果当前顶点是汇点，说明找到一条增广路，得到本条增广路的发送流量incFlow，

​    // 调整des.pre和des正反向边权后返回incFlow

​    **if**(current == des) {

​      Vertex<E> v = current.getPre();

​      incFlow = des.getRemainFlow();

​      setEdgeValue(v, current, getEdgeValue(v, current) - incFlow);

​      setEdgeValue(current, v, getEdgeValue(current, v) + incFlow);

​      **return** incFlow;

​    }

​    // 考察当前顶点的邻接顶点，对符合条件的顶点执行dfs

​    **for** (Vertex<E> w : getAdjList(current)) {

​      **if**(w.getLevel() == current.getLevel() + 1 && getEdgeValue(current, w) > 0) {

​        w.setPre(current); // 将current记录为其邻接顶点w的前驱

​        // 设置w的可用流为current的可用流和(current, w)边权中的较小者

​        w.setRemainFlow(Math.*min*(current.getRemainFlow(), getEdgeValue(current, w)));

​        // dfs递归寻找增广路

​        incFlow += multipleAugPathDfs(w, des);

​        // 找到增广路回退后，对回退到的当前顶点，从它的可用流减中减去求当次增广路发送流incFlow

​        current.setRemainFlow(current.getRemainFlow() - incFlow);;

​        // 如下是一个高效优化，当前顶点的流减到0后，直接退出当前for循环，

​        // 即由于当前顶点current已无剩余流可发送，也即不再存在经过current的其他增广路，所以不必再尝试它的其他邻接顶点

​        **if**(current.getRemainFlow() == 0) {

​          **break**;

​        }

​      }

​    }

​    // 上述for循环结束，说明当前顶点current的邻接顶点均已考察完毕，即已

​    // 穷尽经过current节点的增广路，此时的incFlow就是这些增广路的可发送流量的和，

​    // 用incFlow对(curPre, current)和(current, curPre)边权进行调整。

​    Vertex<E> curPre = current.getPre();

​    **if**(curPre != **null**) { // 注意当current为src时，其前驱为null

​      setEdgeValue(curPre, current, getEdgeValue(curPre, current) - incFlow);

​      setEdgeValue(current, curPre, getEdgeValue(current, curPre) + incFlow);

​    }

​     

​    **return** incFlow;

  }

   

  **public** **void** showVertices(List<Vertex<E>> vertices) {

​    **for** (Vertex<E> v : vertices) {

​      System.***out\***.print(v.getName() + " ");

​    }

​    System.***out\***.println();

  }

   

  **public** **void** printPath(Vertex<E> v) {

​    **if**(v.getPre() != **null**) {

​      printPath(v.getPre());

​      System.***out\***.print(" ");

​    }

​    System.***out\***.print(v.getName());

  }

  **public** **int** getPathValue(Vertex<E> v, Vertex<E> w) {

​    **if**(v == w) {

​      **return** 0;

​    }

​    **return** getPathValue(v, w.getPre()) + getEdgeValue(w.getPre(), w);

  }

   

  **public** List<Vertex<E>> getPath(Vertex<E> v, Vertex<E> w) {

​    List<Vertex<E>> res = **new** ArrayList<>();

​    **while**(v != w) {

​      res.add(w);

​      w = w.getPre();

​    }

​    res.add(v);

​    Collections.*reverse*(res);

​    **return** res;

  }

   

}



/**

 \* 以顶点的名字为泛型，使其可以为一个String型顶点或Integer顶点等

 */

**class** Vertex<E>{

   

  **private** E name; // 顶点名

  **private** **int** topoNum; // 拓扑排序序号，用于拓扑排序

  **private** **int** indegree; // 入度，用于拓扑排序

  **private** **int** distance; // 用于赋权最短路径算法，表示出发点s到该顶点的距离

  **private** **boolean** known; // 是否被访问过的标记

  **private** Vertex<E> pre; // 前驱顶点

  **private** **int** level; // 顶点层号，用于dinic算法

  **private** **int** remainFlow; // 顶点剩余可发送流，用于dinic算法多路增广优化版本

  **private** **boolean** isBlock; // 顶点是否阻塞，用于dinic算法多路增广优化版本，true表示已穷尽经过该顶点的所有可能的增广路

  **public** **final** **static** **int** ***DEFAULT_DIST\*** = -1; // 顶点的初始距离（到源点的距离）

   

  **public** Vertex() {

​    clear();

  }

   

  **public** Vertex(E name) {

​    **this**.name = name;

​    clear();

  }

  **public** **void** clear() {

​    **this**.indegree = 0;

​    **this**.distance = ***DEFAULT_DIST\***;

​    **this**.known = **false**;

​    **this**.topoNum = -1;

​    **this**.level = -1;

​    **this**.pre = **null**;

​    **this**.remainFlow = 0;

​    **this**.isBlock = **false**;

  }

  **public** **int** getIndegree() {

​    **return** **this**.indegree;

  }

  **public** **void** setIndegree(**int** num) {

​    **this**.indegree = num;

  }

  **public** E getName() {

​    **return** name;

  }

  **public** **void** setName(E name) {

​    **this**.name = name;

  }

  **public** **int** getTopoNum() {

​    **return** topoNum;

  }

  **public** **void** setTopoNum(**int** topoNum) {

​    **this**.topoNum = topoNum;

  }

  **public** **int** getDistance() {

​    **return** distance;

  }

  **public** **void** setDistance(**int** distance) {

​    **this**.distance = distance;

  }

  **public** **boolean** isKnown() {

​    **return** known;

  }

  **public** **void** setKnown(**boolean** known) {

​    **this**.known = known;

  }

  **public** Vertex<E> getPre() {

​    **return** pre;

  }

  **public** **void** setPre(Vertex<E> path) {

​    **this**.pre = path;

  }

  **public** **int** getLevel() {

​    **return** level;

  }

  **public** **void** setLevel(**int** level) {

​    **this**.level = level;

  }

  **public** **int** getRemainFlow() {

​    **return** remainFlow;

  }

  **public** **void** setRemainFlow(**int** remainFlow) {

​    **this**.remainFlow = remainFlow;

  }

  **public** **boolean** isBlocked() {

​    **return** isBlock;

  }

  **public** **void** setBlock(**boolean** isBlock) {

​    **this**.isBlock = isBlock;

  }

   

   

}