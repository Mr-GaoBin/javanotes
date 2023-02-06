# hash算法源码解析



```java
///返回内存地址    
public static int hashCode(Object a[]) {
        if (a == null)
            return 0;

        int result = 1;

        for (Object element : a)
            result = 31 * result + (element == null ? 0 : element.hashCode());

        return result;
    }
```

String重写hashcode方法

```java
///返回hash值   
public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }
```







<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202211252307109.png" alt="image-20221125230727982" style="zoom: 67%;" />

​																													（图为hashMap结构）

##  HashMap 的 hash 算法的实现原理（为什么右移 16 位，为什么要使用 ^ 位异或）





hashMap的hash方法

```java
    static final int hash(Object key) {    
        int h;    
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }  


final HashMap.NodegetNode(int hash, Object key) {   
    HashMap.Node[] tab; HashMap.Nodefirst, e;
    int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        // 我们需要关注下面这一行 
        (first = tab[(n - 1) & hash]) != null) {
        if (first.hash == hash && 
            // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        if ((e = first.next) != null) {
            if (first instanceof HashMap.TreeNode)
                return ((HashMap.TreeNode)first).getTreeNode(hash, key);
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;         } while ((e = e.next) != null);
        }
    }   return null;
}
```









