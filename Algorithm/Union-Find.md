# Union-Find

> 

- 

### **응용**
1. 

### **Code**
```
public static void makeSet() {
	for (int i = 1; i < N+1; i++) {
		parent[i] = i;
	}
}
public static int find(int a) {
	if(a == parent[a]) return a;
	return parent[a] = find(parent[a]);
}
public static boolean union(int a, int b) {
	int aRoot = find(a);
	int bRoot = find(b);
	if (aRoot == bRoot) return false;
	if (a < b) {
		parent[bRoot] = aRoot;
	} else 
		parent[aRoot] = bRoot;
	return true;
}
```
