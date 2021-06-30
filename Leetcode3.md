# 1.DFS

## [面试题 04.04. 检查平衡性](https://leetcode-cn.com/problems/check-balance-lcci/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root==null) return true;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode cur = q.poll();
            if(Math.abs(calHeight(cur.left)-calHeight(cur.right))>1) return false;
            if(cur.left!=null) q.offer(cur.left);
            if(cur.right!=null) q.offer(cur.right);
        }
        return true;
    }
    public int calHeight(TreeNode cur){
        if(cur==null) return 0;
        int lh = calHeight(cur.left);
        int rh = calHeight(cur.right);
        int depth = 1 + Math.max(lh,rh);
        return depth;
    }
}

//前序遍历
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root==null) return true;
        else return Math.abs(height(root.left)-height(root.right))<=1 && isBalanced(root.left) && isBalanced(root.right);
    }

    public int height(TreeNode root) {
        if(root==null) return 0;
        else{
            return Math.max(height(root.left),height(root.right))+1;
        }
    }
}

//后序
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root==null) return true;
        else return isBalanced(root.left) && isBalanced(root.right) && Math.abs(height(root.left)-height(root.right))<=1;
    }

    public int height(TreeNode root) {
        if(root==null) return 0;
        else{
            return Math.max(height(root.left),height(root.right))+1;
        }
    }
}

//中序
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root==null) return true;
        else return isBalanced(root.left) && Math.abs(height(root.left)-height(root.right))<=1 && isBalanced(root.right);
    }

    public int height(TreeNode root) {
        if(root==null) return 0;
        else{
            return Math.max(height(root.left),height(root.right))+1;
        }
    }
}

class Solution {
    public boolean isBalanced(TreeNode root) {
        return height(root) >= 0;
    }

    public int height(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftHeight = height(root.left);
        int rightHeight = height(root.right);
        if (leftHeight == -1 || rightHeight == -1 || Math.abs(leftHeight - rightHeight) > 1) {
            return -1;
        } else {
            return Math.max(leftHeight, rightHeight) + 1;
        }
    }
}
```

## [872. 叶子相似的树](https://leetcode-cn.com/problems/leaf-similar-trees/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    
    public boolean leafSimilar(TreeNode root1, TreeNode root2) {
        List<Integer> list1 = new ArrayList<>();
        List<Integer> list2 = new ArrayList<>();
        preOrder(root1,list1);
        preOrder(root2,list2);
        if(list1.size()!=list2.size()) return false;
        for(int i=0;i<list1.size();i++){
            if(list1.get(i)!=list2.get(i)){
                return false;
            }
        }
        return true;
    }
    public void preOrder(TreeNode root,List<Integer> list){
        if(root==null) return ;
        else{
            if(root.left==null && root.right==null) list.add(root.val);
            preOrder(root.left,list);
            preOrder(root.right,list);
        }
    }
}
```

## [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

class Solution {
    List<String> ans = new LinkedList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        preOrder(root,"");
        return ans;
    }

    public void preOrder(TreeNode root,String s){
        if(root==null) return;
        StringBuffer sb = new StringBuffer(s);
        if(root.left==null && root.right==null){
            sb.append(root.val);
            ans.add(sb.toString());
        }
        else{
            sb.append(root.val);
            sb.append("->");
        }
        preOrder(root.left,sb.toString());
        preOrder(root.right,sb.toString());
    }
}

class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        List<String> t = new ArrayList<>();
        preOrder(root,ans,t);
        return ans;
    }
    public void preOrder(TreeNode root,List<String> ans,List<String> t){
        if(root==null){
            return ;
        }
        else{
            t.add(String.valueOf(root.val));
            if(root.left==null && root.right==null){
                StringBuffer sb = new StringBuffer();
                for(int i=0;i<t.size();i++)
                {
                    if(i>0) sb.append("->");
                    sb.append(t.get(i));
                }
                ans.add(sb.toString());
            }
            preOrder(root.left,ans,t);
            preOrder(root.right,ans,t);
            // System.out.println(t);
            //t从主函数传过来的，且不是普通变量，想当于指针，会一直改变，无法回溯
            //但若是string,int这个类型，就算是从主函数传过来的，也能回溯，它传过来的只是一个变量副本
            //可以这样回溯，也可以按照下面那个算法回溯
            t.remove(t.size()-1);
        }
    }
}

class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        List<String> t = new ArrayList<>();
        preOrder(root,ans,t);
        return ans;
    }
    public void preOrder(TreeNode root,List<String> ans,List<String> t){
        if(root==null){
            return ;
        }
        else{
            t.add(String.valueOf(root.val));
            if(root.left==null && root.right==null){
                StringBuffer sb = new StringBuffer();
                for(int i=0;i<t.size();i++)
                {
                    if(i>0) sb.append("->");
                    sb.append(t.get(i));
                }
                ans.add(sb.toString());
            }
            if(root.left!=null){
                preOrder(root.left,ans,t);
                t.remove(t.size()-1);
            }
            if(root.right!=null){
                preOrder(root.right,ans,t);
                t.remove(t.size()-1);
            }
            // System.out.println(t);
            
        }
    }
}

class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        String t = "";
        preOrder(root,ans,t,0);
        return ans;
    }
   
    //这里直接用StringBuffer是不行的，会发现没有回溯，用t和s都可以回溯
    public void preOrder(TreeNode root,List<String> ans,String t,int s){
        if(root==null){
            return ;
        }
        else{
            // StringBuffer sb = new StringBuffer(t);
            // sb.append(String.valueOf(root.val));
            t += root.val;
            s += root.val;
            if(root.left==null && root.right==null){
                // ans.add(sb.toString());
                ans.add(t);
            }
            else{
                // sb.append("->");
                t += "->";
                preOrder(root.left,ans,t,s);
                // System.out.println(t+"--"+s);
                preOrder(root.right,ans,t,s);
            }
        }
    }
}


class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        String t = "";
        preOrder(root,ans,t);
        return ans;
    }
    public void preOrder(TreeNode root,List<String> ans,String t){
        if(root==null){
            return;
        }
        else{
            StringBuffer sb = new StringBuffer(t);
            sb.append(String.valueOf(root.val));
            if(root.left==null && root.right==null){
                ans.add(sb.toString());
            }
            else{
                sb.append("->");
                preOrder(root.left,ans,sb.toString());
                preOrder(root.right,ans,sb.toString());
            }
        }
    }
}

//队列
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> paths = new ArrayList<String>();
        if (root == null) {
            return paths;
        }
        Queue<TreeNode> nodeQueue = new LinkedList<TreeNode>();
        Queue<String> pathQueue = new LinkedList<String>();

        nodeQueue.offer(root);
        pathQueue.offer(Integer.toString(root.val));

        while (!nodeQueue.isEmpty()) {
            TreeNode node = nodeQueue.poll(); 
            String path = pathQueue.poll();

            if (node.left == null && node.right == null) {
                paths.add(path);
            } else {
                if (node.left != null) {
                    nodeQueue.offer(node.left);
                    pathQueue.offer(new StringBuffer(path).append("->").append(node.left.val).toString());
                }

                if (node.right != null) {
                    nodeQueue.offer(node.right);
                    pathQueue.offer(new StringBuffer(path).append("->").append(node.right.val).toString());
                }
            }
        }
        return paths;
    }
}

//迭代法--栈
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        stack<TreeNode*> treeSt;// 保存树的遍历节点
        stack<string> pathSt;   // 保存遍历路径的节点
        vector<string> result;  // 保存最终路径集合
        if (root == NULL) return result;
        treeSt.push(root);
        pathSt.push(to_string(root->val));
        while (!treeSt.empty()) {
            TreeNode* node = treeSt.top(); treeSt.pop(); // 取出节点 中
            string path = pathSt.top();pathSt.pop();    // 取出该节点对应的路径
            if (node->left == NULL && node->right == NULL) { // 遇到叶子节点
                result.push_back(path);
            }
            if (node->right) { // 右
                treeSt.push(node->right);
                pathSt.push(path + "->" + to_string(node->right->val));
            }
            if (node->left) { // 左
                treeSt.push(node->left);
                pathSt.push(path + "->" + to_string(node->left->val));
            }
        }
        return result;
    }
};
```

## [129. 求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)

```java
class Solution {
    int ans = 0;
    public int sumNumbers(TreeNode root) {
        preOrder(root,0);
        return ans;
    }

    public void preOrder(TreeNode root,int s){
        if(root==null) return;
        s = s*10 + root.val;
        if(root.left==null && root.right==null){
            // System.out.println(s);
            ans += s;
        }
        preOrder(root.left,s);
        preOrder(root.right,s);
    }
}
```

## [897. 递增顺序查找树](https://leetcode-cn.com/problems/increasing-order-search-tree/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode increasingBST(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        midOrder(root,list);
        TreeNode t = new TreeNode(list.get(0));
        TreeNode h = t;
        for(int i=1;i<list.size();i++){
            TreeNode cur = new TreeNode(list.get(i));
            t.right = cur;
            t.left = null;
            t = cur;
        }
        return h;
    }

    public void midOrder(TreeNode root,List<Integer> list){
        if(root!=null){
            midOrder(root.left,list);
            list.add(root.val);
            midOrder(root.right,list);
        }
    }
}

class Solution {
    TreeNode cur;
    public TreeNode increasingBST(TreeNode root) {
        TreeNode h = new TreeNode(0);
        cur = h;
        inOrder(root);
        return h.right;
    }

    public void inOrder(TreeNode root){
        if(root!=null){
            inOrder(root.left);
            root.left = null;
            cur.right = root;
            cur = root;
            inOrder(root.right);
        }
    }
}
```

## [563. 二叉树的坡度](https://leetcode-cn.com/problems/binary-tree-tilt/)

```java
class Solution {
    int ans = 0,x = 0;
    public int findTilt(TreeNode root) {
        if(root==null) return 0;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode t = q.poll();
            x = 0;
            int l = postOrder(t.left);
            x = 0;
            int r = postOrder(t.right);
            ans += Math.abs(l-r);
            // System.out.println(l+"--"+r+"--"+t.val);
            if(t.left!=null) q.offer(t.left);
            if(t.right!=null) q.offer(t.right);
        }
        return ans;
    }

    public int postOrder(TreeNode root){
        if(root==null) return 0;
        x += root.val;
        postOrder(root.left);
        postOrder(root.right);
        return x;
    }
}

class Solution {
    int ans = 0;
    public int findTilt(TreeNode root) {
        postOrder(root);
        return ans;
    }

    public int postOrder(TreeNode root){
        if(root==null) return 0;
        int l = postOrder(root.left);
        int r = postOrder(root.right);
        ans += Math.abs(l-r);
        return l+r+root.val;
    }
}
```

## [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return insert(nums,0,nums.length-1);
    }

    public  TreeNode insert(int[] nums,int l,int r){
        if(l>r) return null;
        int mid = (l+r)/2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = insert(nums,l,mid-1);
        root.right = insert(nums,mid+1,r);
        return root;
    }
}
```

## [面试题 16.19. 水域大小](https://leetcode-cn.com/problems/pond-sizes-lcci/)

```java
class Solution {
    int[] X = {0,0,1,-1,1,-1,-1,1};
    int[] Y = {1,-1,0,0,1,-1,1,-1};
    int m,n;
    public int[] pondSizes(int[][] land) {
        List<Integer> list = new ArrayList<>() ;
        m = land.length;
        n = land[0].length;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(land[i][j]==0){
                    int t = BFS(i,j,land);
                    // System.out.println(t);
                    list.add(t);
                }
            }
        }
        int[] ans = new int[list.size()];
        for(int i=0;i<list.size();i++){
            ans[i] = list.get(i);
        }
        return ans;
    }

    public int BFS(int x,int y,int[][] land){
        Queue<Node> q = new LinkedList<>();
        q.offer(new Node(x,y));
        land[x][y] = -1;
        int s = 0;
        while(!q.isEmpty()){
            Node t = q.poll();
            s++;
            for(int i=0;i<8;i++){
                int newX = t.x + X[i];
                int newY = t.y + Y[i];
                if(newX<0 || newX>=m || newY<0 || newY>=n || land[newX][newY]!=0) continue;
                q.offer(new Node(newX,newY));
                // System.out.println(newX+"--"+newY);
                land[newX][newY] = -1;
            }
        }
        return s;
    }
}
class Node{
    int x;
    int y;
    public Node(){}
    public Node(int x,int y){
        this.x = x;
        this.y = y;
    }
}

class Solution {
    int[] X = {0,0,1,-1,1,-1,-1,1};
    int[] Y = {1,-1,0,0,1,-1,1,-1};
    int m,n;
    public int[] pondSizes(int[][] land) {
        List<Integer> list = new ArrayList<>() ;
        m = land.length;
        n = land[0].length;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(land[i][j]==0){
                    int t = DFS(land,i,j);
                    if(t>0) list.add(t);
                }
            }
        }
        int[] ans = list.stream().mapToInt(Integer::valueOf).toArray();
        return ans;
    }

    public int DFS(int[][] land,int x,int y){
        int num = 0;
        if(x<0 || x>=m || y<0 || y>=n || land[x][y]!=0) return num;
        num++;
        land[x][y] = -1;
        for(int i=0;i<8;i++){
            num += DFS(land,x+X[i],y+Y[i]);
        }
        return num;
    }
}
```

## [529. 扫雷游戏](https://leetcode-cn.com/problems/minesweeper/)

```java
class Solution {
    int X[] = {0,0,1,-1,1,-1,1,-1};
    int Y[] = {1,-1,0,0,1,-1,-1,1};
    int n,m;
    // boolean[][] vis;
    public char[][] updateBoard(char[][] board, int[] click) {
        n = board.length;
        m = board[0].length;
        // vis = new boolean[n][m];
        if(board[click[0]][click[1]]=='M'){
            board[click[0]][click[1]] = 'X';  
        }
        else{
            DFS(board,click[0],click[1]);
        }
        return board;
    }

    public void DFS(char[][] board,int x,int y){
        int cnt = 0;
        boolean flag = false;
        for(int i=0;i<8;i++){
            int newX = x+X[i];
            int newY = y+Y[i];
            if(newX<0 || newX>=n || newY<0 || newY>=m ) continue;
            if(board[newX][newY]=='M'){
                cnt++;
            }
        }
        if(cnt>0) board[x][y] = (char)(cnt+'0');
        else{
            board[x][y] = 'B';
            for(int i=0;i<8;i++){
                int newX = x+X[i];
                int newY = y+Y[i];
                if(newX<0 || newX>=n || newY<0 || newY>=m || board[newX][newY]!='E') continue;
                DFS(board,newX,newY);
                /*等价于
                if (tx < 0 || tx >= board.length || ty < 0 || ty >= board[0].length || board[tx][ty] != 'E' || vis[tx][ty]) {
                    continue;
                }
                dfs(board, tx, ty);
                vis[tx][ty] = true;
                */
            }
        }
    }   
}

class Solution {
    int[] dirX = {0, 1, 0, -1, 1, 1, -1, -1};
    int[] dirY = {1, 0, -1, 0, 1, -1, 1, -1};

    public char[][] updateBoard(char[][] board, int[] click) {
        int x = click[0], y = click[1];
        if (board[x][y] == 'M') {
            // 规则 1
            board[x][y] = 'X';
        } else{
            bfs(board, x, y);
        }
        return board;
    }

    public void bfs(char[][] board, int sx, int sy) {
        Queue<int[]> queue = new LinkedList<int[]>();
        boolean[][] vis = new boolean[board.length][board[0].length];
        queue.offer(new int[]{sx, sy});
        vis[sx][sy] = true;
        while (!queue.isEmpty()) {
            int[] pos = queue.poll();
            int cnt = 0, x = pos[0], y = pos[1];
            for (int i = 0; i < 8; ++i) {
                int tx = x + dirX[i];
                int ty = y + dirY[i];
                if (tx < 0 || tx >= board.length || ty < 0 || ty >= board[0].length) {
                    continue;
                }
                // 不用判断 M，因为如果有 M 的话游戏已经结束了
                if (board[tx][ty] == 'M') {
                    ++cnt;
                }
            }
            if (cnt > 0) {
                // 规则 3
                board[x][y] = (char) (cnt + '0');
            } else {
                // 规则 2
                board[x][y] = 'B';
                for (int i = 0; i < 8; ++i) {
                    int tx = x + dirX[i];
                    int ty = y + dirY[i];
                    // 这里不需要在存在 B 的时候继续扩展，因为 B 之前被点击的时候已经被扩展过了
                    if (tx < 0 || tx >= board.length || ty < 0 || ty >= board[0].length || board[tx][ty] != 'E' || vis[tx][ty]) {
                        continue;
                    }
                    queue.offer(new int[]{tx, ty});
                    vis[tx][ty] = true;
                }
            }
        }
    }
}
```

## [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

```java
class Solution {
    List<Integer> list = new ArrayList<>();
    int s = 0,x;
    public TreeNode convertBST(TreeNode root) {
        inOrder(root);
        // System.out.println(list);
        x = list.size()-1;
        inOrder2(root);
        return root;
    }
    public void inOrder(TreeNode root){
        if(root==null) return;
        else{
            inOrder(root.right);
            s += root.val;
            list.add(s);
            inOrder(root.left);
        }
    }
    public void inOrder2(TreeNode root){
        if(root!=null){
            inOrder2(root.left);
            root.val = list.get(x);
            x--;
            inOrder2(root.right);
        }
    }
}

class Solution {
    int s = 0;
    public TreeNode convertBST(TreeNode root) {
        inOrder(root);
        return root;
    }
    public void inOrder(TreeNode root){
        if(root==null) return;
        else{
            inOrder(root.right);
            s += root.val;
            root.val = s;
            inOrder(root.left);
        }
    }
}

class Solution {
    int s = 0;
    public TreeNode convertBST(TreeNode root) {
        if(root==null) return null;
        else{
            convertBST(root.right);
            s += root.val;
            root.val = s;
            convertBST(root.left);
        }
        return root;
    }
}
```

## [547. 省份数量](https://leetcode-cn.com/problems/number-of-provinces/)

```java
class Solution {
    boolean[] vis;
    int n,m;
    public int findCircleNum(int[][] isConnected) {
        n = isConnected.length;
        m = isConnected[0].length;
        vis = new boolean[n];
        int cnt = 0;
        for(int i=0;i<n;i++){
            if(!vis[i]){
                dfs(i,isConnected);
                cnt++;
            }
        }
        return cnt;
    }
    public void dfs(int u,int[][] isConnected){
        vis[u] = true;
        for(int i=0;i<n;i++){
            if(isConnected[u][i]==1 && !vis[i]){
                dfs(i,isConnected);
            }
        }
    }
}

class Solution {
    int[] father;
    int n,m;
    public int findCircleNum(int[][] isConnected) {
        int cnt = 0;
        n = isConnected.length;
        m = isConnected[0].length;
        father = new int[n];
        for(int i=0;i<n;i++) father[i] = i;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(isConnected[i][j]==1){
                    union(i,j);
                }
            }
        }  
        for(int i=0;i<n;i++){
            if(father[i]==i) cnt++;
        }
        return cnt;
    }

    int findFather(int x){
        int a = x;
        while(x!=father[x]){
            x = father[x];
        }
        while(a!=father[a]){
            int z = a;
            a = father[a];
            father[z] = x;
        }
        return x;
    }

    void union(int a,int b){
        int fa = findFather(a);
        int fb = findFather(b);
        if(fa!=fb){
            father[fa] = fb;
        }
    }
}
```

## [面试题 17.07. 婴儿名字](https://leetcode-cn.com/problems/baby-names-lcci/)

```java
class Solution {
    boolean[] vis;
    int s = 0,x = 0;
    Map<String,Integer> map = new HashMap<>();
    Map<Integer,Node> m2 = new HashMap<>();
    ArrayList<Integer>[] G;
    public String[] trulyMostPopular(String[] names, String[] synonyms) {
        int n = names.length;  
        int m = synonyms.length;  
        G = new ArrayList[2*m+n];
        vis = new boolean[2*m+n];
        for(int i=0;i<n;i++){
            int r = names[i].indexOf('(');
            int t = Integer.parseInt(names[i].substring(r+1,names[i].length()-1));
            map.put(names[i].substring(0,r),i);
            m2.put(i,new Node(t,names[i].substring(0,r)));
        }
        for(int i=0;i<2*m+n;i++) G[i] = new ArrayList<>();
        // System.out.println(map);
        
        for(int i=0;i<m;i++){
            int r = synonyms[i].indexOf(',');
            // System.out.println(r+"--"+synonyms[i].substring(1,r)+"--"+synonyms[i].substring(r+1,synonyms[i].length()-1));
            int a = 0;
            int b = 0;
            if(map.get(synonyms[i].substring(1,r))!=null){
                a = map.get(synonyms[i].substring(1,r));
            }
            else{
                a = n++;
                m2.put(a,new Node(0,synonyms[i].substring(1,r)));
                map.put(synonyms[i].substring(1,r),a);
            }
            if(map.get(synonyms[i].substring(r+1,synonyms[i].length()-1))!=null){
                b = map.get(synonyms[i].substring(r+1,synonyms[i].length()-1));
            }
            else{
                b = n++;
                m2.put(b,new Node(0,synonyms[i].substring(r+1,synonyms[i].length()-1)));    
                map.put(synonyms[i].substring(r+1,synonyms[i].length()-1),b);
            }
            // int b = 0;
            System.out.println(synonyms[i]+"--"+a+"--"+b);
            G[a].add(b);
            G[b].add(a);
        }
        List<String> ans = new ArrayList<>();
        for(int i=0;i<n;i++){
            if(!vis[i]){
                s = 0;
                x = i;
                dfs(i);
                ans.add(m2.get(x).v+"("+s+")");
            }
        }
        // System.out.println(ans);
        // System.out.println(ans.toArray(new String[ans.size()]));
        return ans.toArray(new String[ans.size()]);
    }

    public void dfs(int v){
        vis[v] = true;
        // System.out.println(v);
        s += m2.get(v).k;
        if(m2.get(v).v.compareTo(m2.get(x).v)<0) x = v;
        for(int i=0;i<G[v].size();i++){
            int u = G[v].get(i);
            if(!vis[u])
                dfs(u);
        }
    }
}

class Node{
    int k;
    String v;
    public Node(){}
    public Node(int k,String v){
        this.k = k;
        this.v = v;
    }
}

class Solution {
    public String[] trulyMostPopular(String[] names, String[] synonyms) {
        Map<String, Integer> map = new HashMap<>();
        Map<String, String> unionMap = new HashMap<>();
        for (String name : names) {     //统计频率
            int idx1 = name.indexOf('(');
            int idx2 = name.indexOf(')');
            int frequency = Integer.valueOf(name.substring(idx1 + 1, idx2));
            map.put(name.substring(0, idx1), frequency);
        }
        for (String pair : synonyms) {  //union同义词
            int idx = pair.indexOf(',');
            String name1 = pair.substring(1, idx);
            String name2 = pair.substring(idx + 1, pair.length() - 1);
            while (unionMap.containsKey(name1)) {   //找name1祖宗
                name1 = unionMap.get(name1);
            }
            while (unionMap.containsKey(name2)) {   //找name2祖宗
                name2 = unionMap.get(name2);
            }
            if(!name1.equals(name2)){   //祖宗不同，要合并
                int frequency = map.getOrDefault(name1, 0) + map.getOrDefault(name2, 0);    //出现次数是两者之和
                String trulyName = name1.compareTo(name2) < 0 ? name1 : name2;
                String nickName = name1.compareTo(name2) < 0 ? name2 : name1;
                unionMap.put(nickName, trulyName);      //小名作为大名的分支，即大名是小名的祖宗
                map.remove(nickName);       //更新一下数据
                map.put(trulyName, frequency);
            }
        }
        String[] res = new String[map.size()];
        int index = 0;
        for (String name : map.keySet()) {
            StringBuilder sb = new StringBuilder(name);
            sb.append('(');
            sb.append(map.get(name));
            sb.append(')');
            res[index++] = sb.toString();
        }
        return res;
    }
}

class Solution {
    public String[] trulyMostPopular(String[] names, String[] synonyms) {
        UnionFind uf = new UnionFind();
        for(String str : names) {
            int index1 = str.indexOf('('), index2 = str.indexOf(')');
            String name = str.substring(0, index1);
            int count = Integer.valueOf(str.substring(index1 + 1, index2));
            //并查集初始化
            uf.parent.put(name, name);
            uf.size.put(name, count);

        }
        for(String synonym : synonyms) {
            int index = synonym.indexOf(',');
            String name1 = synonym.substring(1, index);
            String name2 = synonym.substring(index + 1, synonym.length() - 1);
            //避免漏网之鱼
            if(!uf.parent.containsKey(name1)) {
                uf.parent.put(name1, name1);
                //注意人数为0
                uf.size.put(name1, 0);
            }
            if(!uf.parent.containsKey(name2)) {
                uf.parent.put(name2, name2);
                uf.size.put(name2, 0);
            }
            uf.union(name1, name2);
        } 
        List<String> res = new ArrayList<>();
        for(String str : names) {
            int index1 = str.indexOf('('), index2 = str.indexOf(')');
            String name = str.substring(0, index1);
            //根节点
            if(name.equals(uf.find(name))) 
                res.add(name + "(" + uf.size.get(name) + ")");    
        }
         return res.toArray(new String[res.size()]);
    }
}

//并查集
//路径压缩
public class UnionFind{
    //当前节点的父亲节点
    Map<String, String> parent;
    //当前节点人数
    Map<String, Integer> size;

    public UnionFind() {
        this.parent = new HashMap<>();
        this.size = new HashMap<>();
    }

    //找到x的根节点
    public String find(String x) {
        if(parent.get(x).equals(x))
            return x;
        //路径压缩
        parent.put(x, find(parent.get(x)));
        return parent.get(x);
    }

    public void union(String x, String y) {
        String str1 = find(x), str2 = find(y);
        if(str1.equals(str2))
            return;
        //字典序小的作为根
        if(str1.compareTo(str2) > 0) {
            parent.put(str1, str2);
            //人数累加到根节点
            size.put(str2, size.get(str1) + size.get(str2));
        }else {
            parent.put(str2, str1);
            size.put(str1, size.get(str2) + size.get(str1));
        }
    }
}
```

# BFS

## [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)

```java
class Solution {
    public Node connect(Node root) {
        if(root==null) return null;
        Queue<Node> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            int size = q.size();
            for(int i=0;i<size;i++){
                Node t = q.poll();
                if(i<size-1){
                    Node tt = q.peek();
                    t.next = tt;
                }
                if(t.left!=null) q.offer(t.left);
                if(t.right!=null) q.offer(t.right);
            } 
        }
        return root;
    }
}

class Solution {
    public Node connect(Node root) {
        if(root==null || root.left==null) return root;
        root.left.next = root.right;
        if(root.next!=null) root.right.next = root.next.left;
        connect(root.left);
        connect(root.right);
        return root;
    }
}
```

# 图的应用

## [802. 找到最终的安全状态](https://leetcode-cn.com/problems/find-eventual-safe-states/)

```java
class Solution {
    int flag = 0;
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        List<Integer> list = new ArrayList<>();
        int[] vis = new int[n];
        for(int i=0;i<n;i++){
            if(DFS(i,vis,graph)){
                list.add(i);
            }
     
        }
        return list;
    }

    public boolean DFS(int v,int[] vis,int[][] graph){
        if(vis[v]==2) return true;

        vis[v] = 1;
        for(int i=0;i<graph[v].length;i++){
            if(vis[graph[v][i]]==1) return false;
            else if(vis[graph[v][i]]==0){
                if(!DFS(graph[v][i],vis,graph)) return false;
            }
        }
        vis[v] = 2;

        return true;
    }
}

class Solution {
    public  List<Integer> eventualSafeNodes(int[][] G) {
        int n = G.length;
        int[] inDegree = new int[n];
        Queue<Integer> q = new LinkedList<>();
        List<Integer> list = new ArrayList<>();
        List<Integer>[] newG = new ArrayList[n];
        for(int i=0;i<n;i++){
            inDegree[i] = G[i].length;
            newG[i] = new ArrayList<>();
        }
        for(int i=0;i<n;i++){
            for(int j=0;j<G[i].length;j++){
                int v = G[i][j];
                newG[v].add(i);
            }
        }
        for(int i=0;i<inDegree.length;i++){
            if(inDegree[i]==0) q.offer(i);
        }
        
        while(!q.isEmpty()){
            int u = q.poll();
            list.add(u);
            for(int i=0;i<newG[u].size();i++){
                int v = newG[u].get(i);
                inDegree[v]--;
                if(inDegree[v]==0) q.offer(v);
            }
        }
        Collections.sort(list);
        return list;
    }
}

class Solution {
    public List<Integer> eventualSafeNodes(int[][] G) {
        int N = G.length;
        boolean[] safe = new boolean[N];

        List<Set<Integer>> graph = new ArrayList();
        List<Set<Integer>> rgraph = new ArrayList();
        for (int i = 0; i < N; ++i) {
            graph.add(new HashSet());
            rgraph.add(new HashSet());
        }

        Queue<Integer> queue = new LinkedList();

        for (int i = 0; i < N; ++i) {
            if (G[i].length == 0)
                queue.offer(i);
            for (int j: G[i]) {
                graph.get(i).add(j);
                rgraph.get(j).add(i);
            }
        }

        while (!queue.isEmpty()) {
            int j = queue.poll();
            safe[j] = true;
            for (int i: rgraph.get(j)) {
                graph.get(i).remove(j);
                if (graph.get(i).isEmpty())
                    queue.offer(i);
            }
        }

        List<Integer> ans = new ArrayList();
        for (int i = 0; i < N; ++i) if (safe[i])
            ans.add(i);

        return ans;
    }
}

```

## [1387. 将整数按权重排序](https://leetcode-cn.com/problems/sort-integers-by-the-power-value/)

```java
class Solution {
    public int getKth(int lo, int hi, int k) {
        List<Node> list = new ArrayList<>();
        for(int i=lo;i<=hi;i++){
            list.add(new Node(getCnt(i),i));
        }
        Collections.sort(list,new Comparator<Node>(){
            public int compare(Node a,Node b){
                if(a.k==b.k) return a.v-b.v;
                return a.k-b.k;
            }
        });
        return list.get(k-1).v;
    }

    public int getCnt(int x){
        int cnt = 0;
        while(x!=1){
            if(x%2==0) x = x/2;
            else x = 3*x+1;
            cnt++;
        }
        return cnt;
    }

    class Node{
        int k,v;
        public Node(int k,int v){
            this.k = k;
            this.v = v;
        }
    }
}

class Solution {
    public int getKth(int lo, int hi, int k) {
        List<Integer> list = new ArrayList<>();
        for(int i=lo;i<=hi;i++){
            list.add(i);
        }
        Collections.sort(list,new Comparator<Integer>(){
            public int compare(Integer a,Integer b){
                if(getCnt(a)!=getCnt(b)) return getCnt(a)-getCnt(b);
                return a-b;
            }
        });
        return list.get(k-1);
    }

    public int getCnt(int x){
        int cnt = 0;
        while(x!=1){
            if(x%2==0) x = x/2;
            else x = 3*x+1;
            cnt++;
        }
        return cnt;
    }

    public int getCnt(int x){
        if(x==1) return 0;
        else if(x%2==0) return getCnt(x/2)+1;
        else return getCnt(3*x+1)+1;
    }
}
```



# 2.二叉树01

## 前序遍历

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        Deque<TreeNode> stack = new LinkedList<>();
        while(root!=null || !stack.isEmpty()){
            while(root!=null){
                ans.add(root.val);
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            root = root.right;
        }
        return ans;
    }
}

class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();                      // 中
            st.pop();
            if (node != NULL) result.push_back(node->val);
            else continue;
            st.push(node->right);                           // 右
            st.push(node->left);                            // 左
        }
        return result;
    }
};
```

## 中序遍历

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        Deque<TreeNode> stk = new LinkedList<TreeNode>();
        while (root != null || !stk.isEmpty()) {
            while (root != null) {
                stk.push(root);
                root = root.left;
            }
            root = stk.pop();
            res.add(root.val);
            root = root.right;
        }
        return res;
    }
}

class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        TreeNode* cur = root;
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) { // 指针来访问节点，访问到最底层
                st.push(cur); // 讲访问的节点放进栈
                cur = cur->left;                // 左
            } else {
                cur = st.top(); // 从栈里弹出的数据，就是要处理的数据（放进result数组里的数据）
                st.pop();
                result.push_back(cur->val);     // 中
                cur = cur->right;               // 右
            }
        }
        return result;
    }
};
```

## 后序遍历

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if (root == null) {
            return res;
        }

        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        TreeNode prev = null;
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if (root.right == null || root.right == prev) {
                res.add(root.val);
                prev = root;
                root = null;
            } else {
                stack.push(root);
                root = root.right;
            }
        }
        return res;
    }
}


如果1： 我们将前序遍历中节点插入结果链表尾部的逻辑，修改为将节点插入结果链表的头部那么结果链表就变为了：右 -> 左 -> 根
如果2： 我们将遍历的顺序由从左到右修改为从右到左，配合如果1那么结果链表就变为了：左 -> 右 -> 根
这刚好是后序遍历的顺序
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> ans = new LinkedList<>();
        Deque<TreeNode> stack = new LinkedList<>();
        while(root!=null || !stack.isEmpty()){
            while(root!=null){
                ans.addFirst(root.val);
                stack.push(root);
                root = root.right;
            }
            root = stack.pop();
            root = root.left;
        }
        return ans;
    }  
}

再来看后序遍历，先序遍历是中左右，后续遍历是左右中，那么我们只需要调整一下先序遍历的代码顺序，就变成中右左的遍历顺序，然后在反转result数组，输出的结果顺序就是左右中了
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            if (node != NULL) result.push_back(node->val);
            else continue;
            st.push(node->left); // 相对于前序遍历，这更改一下入栈顺序
            st.push(node->right);
        }
        reverse(result.begin(), result.end()); // 将结果反转之后就是左右中的顺序了
        return result;
    }
};
```

## 层序遍历

### [107. 二叉树的层序遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        LinkedList<List<Integer>> ans = new LinkedList<>();
        if(root==null) return ans;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            List<Integer> list = new LinkedList<>();
            int s = q.size();
            for(int i=0;i<s;i++){
                TreeNode t = q.poll();
                list.add(t.val);
                if(t.left!=null) q.offer(t.left);
                if(t.right!=null) q.offer(t.right);
            }
            ans.addFirst(list);
        }
        return ans;
    }
}
```

### 199. 二叉树的右视图

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ans = new LinkedList<>();
        if(root==null) return ans;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            int l = q.size();
            for(int i=0;i<l;i++){
                TreeNode t = q.poll();
                if(i==l-1) ans.add(t.val);
                if(t.left!=null) q.offer(t.left);
                if(t.right!=null) q.offer(t.right);
            }
        }
        return ans;
    }
}

class Solution {
    List<Integer> ans = new LinkedList<>();
    public List<Integer> rightSideView(TreeNode root) {
        postOrder(root,0);
        return ans;
    }

    public void postOrder(TreeNode root,int layer){
        if(root==null) return;
        if(layer==ans.size()){
            ans.add(root.val);
        }
        postOrder(root.right,layer+1);
        postOrder(root.left,layer+1);   
    }
}
```

### [637. 二叉树的层平均值](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/)

```java
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> ans = new LinkedList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            int l = q.size();
            double s = 0;
            for(int i=0;i<l;i++){
                TreeNode t = q.poll();
                s += t.val;
                if(t.left!=null) q.offer(t.left);
                if(t.right!=null) q.offer(t.right);
            }
            ans.add(s/l);
        }
        return ans;
    }
}
```

### [429. N 叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        ArrayList<List<Integer>> ans = new ArrayList<>();
        if(root==null) return ans;
        Queue<Node> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            ArrayList<Integer> list = new ArrayList<>();
            int s = q.size();
            for(int i=0;i<s;i++){
                Node t = q.poll();
                list.add(t.val);
                for(Node n:t.children){
                    if(n!=null) q.offer(n);
                }
            }
            ans.add(list);
    }
    return ans;
}
}
```

## [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
//层序
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root==null) return root;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode t = q.poll();
            if(t.left!=null) q.offer(t.left);
            if(t.right!=null) q.offer(t.right);
            TreeNode td = t.left;
            t.left = t.right;
            t.right = td;
        }
        return root;
    }
}

//后序
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root==null) return null;
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
}

//前序
class Solution {
    public TreeNode invertTree(TreeNode root) {
        preOrder(root);
        return root;
    }
    public void preOrder(TreeNode root){
        if (root == null) return;
        TreeNode t = root.left;
        root.left = root.right;
        root.right = t;
        preOrder(root.left);   
        preOrder(root.right);
    }
}
```

# 3.二叉树02

## [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root==null) return true;
        return check(root.left, root.right);
    }

    public boolean check(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }
        if (p == null || q == null) {
            return false;
        }
        return p.val == q.val && check(p.left, q.right) && check(p.right, q.left);
    }
}

class Solution {
    public boolean isSymmetric(TreeNode root) {
        return check(root, root);
    }

    public boolean check(TreeNode u, TreeNode v) {
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.offer(u);
        q.offer(v);
        while (!q.isEmpty()) {
            u = q.poll();
            v = q.poll();
            if (u == null && v == null) {
                continue;
            }
            if ((u == null || v == null) || (u.val != v.val)) {
                return false;
            }

            q.offer(u.left);
            q.offer(v.right);

            q.offer(u.right);
            q.offer(v.left);
        }
        return true;
    }
}

class Solution {
public:
    bool compare(TreeNode* left, TreeNode* right) {
        // 首先排除空节点的情况
        if (left == NULL && right != NULL) return false;
        else if (left != NULL && right == NULL) return false;
        else if (left == NULL && right == NULL) return true;
        // 排除了空节点，再排除数值不相同的情况
        else if (left->val != right->val) return false;

        // 此时就是：左右节点都不为空，且数值相同的情况
        // 此时才做递归，做下一层的判断
        bool outside = compare(left->left, right->right);   // 左子树：左、 右子树：右
        bool inside = compare(left->right, right->left);    // 左子树：右、 右子树：左
        bool isSame = outside && inside;                    // 左子树：中、 右子树：中 （逻辑处理）
        return isSame;

    }
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) return true;
        return compare(root->left, root->right);
    }
};
```

```java
class Solution {
    int ans = 0;
    public int maxDepth(TreeNode root) {
        ans = depth(root);
        System.out.println("ans:"+ans);
        return ans;
    }

    public int depth(TreeNode root){
        if(root!=null){
            depth(root.left);
            depth(root.right);
            ans++;
            System.out.println(root.val+"--"+ans);
            return ans;
        }
        return 0;
    }
}

 	3
   / \
  9  20
    /  \
   15   7
   
 
9--1
15--2
7--3
20--4
3--5
ans:5
```

## [100. 相同的树](https://leetcode-cn.com/problems/same-tree/)

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p==null && q==null) return true;
        if(p==null || q==null) return false;
        return p.val==q.val && isSameTree(p.left,q.left) && isSameTree(p.right,q.right);
    }
}

class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p==null && q==null) return true;
        if(p==null || q==null) return false;
        Queue<TreeNode> q1 = new LinkedList<>();
        Queue<TreeNode> q2 = new LinkedList<>();
        q1.offer(p);
        q2.offer(q);
        while(!q1.isEmpty()&&!q2.isEmpty()){
            TreeNode t1 = q1.poll();
            TreeNode t2 = q2.poll();
            if(t1.val!=t2.val) return false;
            if(t1.left!=null && t2.left!=null){
                q1.offer(t1.left);
                q2.offer(t2.left);
            }
            else if((t1.left==null && t2.left!=null) || (t1.left!=null && t2.left==null)){
                return false;
            }
            if(t1.right!=null && t2.right!=null){
                q1.offer(t1.right);
                q2.offer(t2.right);
            }
            else if((t1.right==null && t2.right!=null) || (t1.right!=null && t2.right==null)){
                return false;
            }
        }
        return true;
    }
}
```

## [572. 另一个树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)

```java
class Solution {
    public boolean isSubtree(TreeNode s, TreeNode t) {
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(s);
        while(!q.isEmpty()){
            TreeNode temp = q.poll();
            if(temp.val==t.val && isSame(temp,t)){
                return true;
            }
            if(temp.left!=null) q.offer(temp.left) ;
            if(temp.right!=null) q.offer(temp.right);
        }
        return false;
    }

    public boolean isSame(TreeNode s,TreeNode t){
        if(s==null && t==null) return true;
        if(s==null || t==null) return false;
        return s.val==t.val && isSame(s.left,t.left) && isSame(s.right,t.right);
    }
}

class Solution {
    public boolean isSubtree(TreeNode s, TreeNode t) {
        if(s==null) return false;
        return isSame(s,t) || isSubtree(s.left,t) || isSubtree(s.right,t);
    }

    public boolean isSame(TreeNode p,TreeNode q){
        if(p==null && q==null) return true;
        if(p==null || q==null) return false;
        return p.val==q.val && isSame(p.left,q.left) && isSame(p.right,q.right);
    }
}
```



## [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

使用前序求的就是深度，使用后序呢求的是高度。

二叉树的深度和高度求法是一样的，因为它们大小相等。具体到某个节点，求深度用前序，求高度用后序

```java
class Solution {
    int ans = 0;
    public int maxDepth(TreeNode root) {
        ans = depth(root,1);
        return ans;
    }

    public int depth(TreeNode root,int d){
        if(root==null) return 0;
        if(root.left==null && root.right==null){
            if(d > ans) ans = d;
        }
        depth(root.left,d+1);
        depth(root.right,d+1);
        return ans;
    }
}

//求高度一般都用这个
class Solution {
    public int maxDepth(TreeNode root) {
        int ans = depth(root);
        return ans;
    }

    public int depth(TreeNode root){
        if(root==null) return 0;
        return Math.max(depth(root.left),depth(root.right))+1;
    }
}
```

## [559. N 叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/
//递归
class Solution {
    public int maxDepth(Node root) {
        int ans = depth(root);
        return ans;
    }

    public int depth(Node root){
        if(root==null) return 0;
        int d = 0;
        for(Node t:root.children){
            d = Math.max(d,depth(t));
        }
        return d+1;
    } 
}

//迭代
class Solution {
    public int maxDepth(Node root) {
        if(root==null) return 0;
        int ans = 0;
        Queue<Node> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            int l = q.size();
            ans++;
            for(int i=0;i<l;i++){
                Node t = q.poll();
                for(Node tt:t.children) q.offer(tt);
            }
        }
        return ans;
    }
}
```

## [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

```java
class Solution {
    int ans = 100001;
    public int minDepth(TreeNode root) {
        if(root==null) return 0;
        ans = depth(root,1);
        return ans;
    }

    public int depth(TreeNode root,int d){
        if(root==null) return 0;
        if(root.left==null && root.right==null){
            if(d<ans) ans = d;
        }
        depth(root.left,d+1);
        depth(root.right,d+1);
        return ans;
    }
}

class Solution {
    public int minDepth(TreeNode root) {
        int ans = depth(root);
        return ans;
    }

    public int depth(TreeNode root){
        if(root==null) return 0;
        else if(root.left==null) return depth(root.right)+1;
        else if(root.right==null) return depth(root.left)+1;
        else return Math.min(depth(root.left),depth(root.right))+1;
    }
}

class Solution {
    public int minDepth(TreeNode root) {
        if(root==null) return 0;
        int ans = 0;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            ++ans;
            int l = q.size();
            for(int i=0;i<l;i++){
                TreeNode t = q.poll();
                if(t.left==null && t.right==null){
                    return ans;
                }
                if(t.left!=null) q.offer(t.left);
                if(t.right!=null) q.offer(t.right);
            }
        }
        return ans;
    }
}
```

## [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

```java
class Solution {
    int ans = 0;
    public int countNodes(TreeNode root) {
        preOrder(root);
        return ans;
    }
    public void preOrder(TreeNode root){
        if(root!=null){
            ans++;
            preOrder(root.left);
            preOrder(root.right);
        }
    }
}

class Solution {
    public int countNodes(TreeNode root) {
        if(root==null) return 0;
        int ans = 0;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode t = q.poll();
            ++ans;
            if(t.left!=null) q.offer(t.left);
            if(t.right!=null) q.offer(t.right);
        }
        return ans;
    }
}

class Solution {
    public int countNodes(TreeNode root) {
        return preOrder(root);
    }
    public int preOrder(TreeNode root){
        if(root==null) return 0;
        return preOrder(root.left) + preOrder(root.right) + 1;
    }
}
```

## [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

见1DFS中面试题

## [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

# 4.二叉树03

## [404. 左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)

```java
class Solution {
    int ans = 0;
    public int sumOfLeftLeaves(TreeNode root) {
        postOrder(root);
        return ans;
    }

    public void postOrder(TreeNode root){
        if(root!=null){
            postOrder(root.left);
            postOrder(root.right);
            //下面这一句放前中后都行
            if(root.left!=null && root.left.left==null && root.left.right==null) ans += root.left.val;
        }
    }
}

class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root==null) return 0;
        int ans = 0;
        if(root.left!=null && root.left.left==null && root.left.right==null) ans += root.left.val;
        return ans + sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);
    }
}

class Solution {   
    public int sumOfLeftLeaves(TreeNode root) {
        if(root==null) return 0;
        int ans = 0;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode t = q.poll();
            if(t.left!=null){
                q.offer(t.left);
                if(t.left.left==null && t.left.right==null) ans += t.left.val;
            }
            if(t.right!=null) q.offer(t.right);
        }
        return ans;
    }
}
```

## [513. 找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

```java
class Solution {
    int ans = 0,level = -1;
    public int findBottomLeftValue(TreeNode root) {
        postOrder(root,0);
        return ans;
    }

    public void postOrder(TreeNode root,int d){
        if(root!=null){
            //这一句放前中后都行，但是放在前面最合适，从低到高
            if(d>level){
                ans = root.val;
                level = d;
            }
            postOrder(root.left,d+1);
            postOrder(root.right,d+1);  
        }
    }
}

class Solution {
    int ans = 0,level = -1;
    public int findBottomLeftValue(TreeNode root) {
        postOrder(root,0);
        return ans;
    }

    public void postOrder(TreeNode root,int d){
        if(root==null) return ;
        if(root.left==null && root.right==null){
            if(d>level){
                ans = root.val;
                level = d;
            }
        }
        postOrder(root.left,d+1);
        postOrder(root.right,d+1);  
    }
}


class Solution {
public:
    int maxLen = INT_MIN;
    int maxleftValue;
    void traversal(TreeNode* root, int leftLen) {
        if (root->left == NULL && root->right == NULL) {
            if (leftLen > maxLen) {
                maxLen = leftLen;
                maxleftValue = root->val;
            }
            return;
        }
        if (root->left) {
            leftLen++;
            traversal(root->left, leftLen);
            leftLen--; // 回溯
        }
        if (root->right) {
            leftLen++;
            traversal(root->right, leftLen);
            leftLen--; // 回溯 
        }
        /*
        if (root->left) {
            traversal(root->left, leftLen + 1); // 隐藏着回溯
        }
        if (root->right) {
            traversal(root->right, leftLen + 1); // 隐藏着回溯
        }
        */
        return;
    }
    int findBottomLeftValue(TreeNode* root) {
        traversal(root, 0);
        return maxleftValue;
    }
};

class Solution {
    public int findBottomLeftValue(TreeNode root) {
        int ans = 0;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            int l = q.size();
            for(int i=0;i<l;i++){
                TreeNode t = q.poll();
                if(i==0) ans = t.val;
                if(t.left!=null) q.offer(t.left);
                if(t.right!=null) q.offer(t.right);
            }
        }
        return ans;
    }
}
```

## [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

```java
class Solution {
    boolean flag = false;
    public boolean hasPathSum(TreeNode root, int targetSum) {
        preOrder(root,0,targetSum);
        return flag;
    }
    public void preOrder(TreeNode root,int s,int target){
        if(root!=null){
            s += root.val;
            if(root.left==null && root.right==null){
                if(s==target){
                    flag = true;
                    return;
                }
            }
            preOrder(root.left,s,target);
            preOrder(root.right,s,target);
           // s -= root.val;s可以回溯不需要减值
        }
    }
}

class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root==null) return false;
        return preOrder(root,targetSum-root.val);
    }
    public boolean preOrder(TreeNode root,int target){
        if(root.left==null && root.right==null && target==0){
            return true;
        }
        if(root.left!=null){
            if(preOrder(root.left,target-root.left.val)) return true;
        }
        if(root.right!=null){
            if(preOrder(root.right,target-root.right.val)) return true;
        }
        return false;
    }
}

class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        return preOrder(root,0,targetSum);
    }
    public boolean preOrder(TreeNode root,int s,int target){
        if(root==null) return false;
        s += root.val;
        if(root.left==null && root.right==null){
            if(s==target) return true;
        }
        if(preOrder(root.left,s,target)) return true;
        if(preOrder(root.right,s,target)) return true;
        return false;
    }
}

class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root==null) return false;
        if(root.left==null && root.right==null){
            return targetSum == root.val;
        }
        return hasPathSum(root.left,targetSum-root.val) || hasPathSum(root.right,targetSum-root.val);
    }    
}

class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root==null) return false;
        Queue<TreeNode> q1 = new LinkedList<>();
        Queue<Integer> q2 = new LinkedList<>();
        q1.offer(root);
        q2.offer(root.val);
        while(!q1.isEmpty()){
            TreeNode t = q1.poll();
            int tt = q2.poll();
            if(t.left==null && t.right==null && tt==targetSum){
                return true;
            }
            if(t.left!=null){
                q1.offer(t.left);
                q2.offer(tt+t.left.val);
            }
            if(t.right!=null){
                q1.offer(t.right);
                q2.offer(t.right.val+tt);
            }
        }
        return false;
    }    
}
```

## [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    LinkedList<List<Integer>> ans = new LinkedList<>();
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        if(root==null) return ans;
        LinkedList<Integer> list = new LinkedList<>();
        preOrder(root,targetSum-root.val,list);
        return ans;
    }

    public void preOrder(TreeNode root,int target,LinkedList<Integer> list){
        if(root==null) return;
        list.add(root.val);
        // System.out.println(root.val+"-"+list+"-"+target);
        if(root.left==null && root.right==null && target==0){
            ans.add(new LinkedList(list));
        }
        if(root.left!=null){
            preOrder(root.left,target-root.left.val,list);
            list.removeLast();
        }
        if(root.right!=null){
            preOrder(root.right,target-root.right.val,list);
            list.removeLast();
        }
    }
}
```

## [654. 最大二叉树](https://leetcode-cn.com/problems/maximum-binary-tree/)

```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return fun(0,nums.length-1,nums);
    }

    public TreeNode fun(int left,int right,int[] nums){
        if(left>right) return null;
        int m = left;
        for(int i=left+1;i<=right;i++){
            if(nums[i]>nums[m]) m = i;
        }
        TreeNode root = new TreeNode(nums[m]);
        root.left = fun(left,m-1,nums);
        root.right = fun(m+1,right,nums);
        return root;
    }
}

class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        if(nums.length==0) return null;
        int m = 0;
        for(int k=0;k<nums.length;k++){
            if(nums[k]>nums[m]){
                m = k;
            }
        }
        TreeNode root = new TreeNode(nums[m]);
        root.left = constructMaximumBinaryTree(Arrays.copyOfRange(nums,0,m));
        root.right = constructMaximumBinaryTree(Arrays.copyOfRange(nums,m+1,nums.length));
        return root;
    }
}
```

# 5.二叉树04

## [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if(root1==null) return root2;
        if(root2==null) return root1;
        Queue<TreeNode> q1 = new LinkedList<>();
        Queue<TreeNode> q2 = new LinkedList<>();
        root1.val += root2.val;
        q1.offer(root1);
        q2.offer(root2);
        while(!q2.isEmpty()){
            TreeNode t1=null,t2=null;
            if(!q1.isEmpty()) t1 = q1.poll();
            if(!q2.isEmpty()) t2 = q2.poll();
            if(t1.left!=null && t2.left!=null) t1.left.val += t2.left.val;
            else if(t1.left==null && t2.left!=null) {
                t1.left = new TreeNode(t2.left.val);
            }
            if(t1.right!=null && t2.right!=null) t1.right.val += t2.right.val;
            else if(t1.right==null && t2.right!=null) {
                t1.right = new TreeNode(t2.right.val);
            }
            // System.out.println(t1.val+"--"+t2.val); 
            if(t2.left!=null){
                q2.offer(t2.left);
                q1.offer(t1.left);
            }
            if(t2.right!=null) {
                q2.offer(t2.right);
                q1.offer(t1.right);
            }
        }
        return root1;
    }
}

class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if(root1==null) return root2;
        if(root2==null) return root1;
        root1.val += root2.val;
        preOrder(root1,root2);
        return root1;
    }

    public void preOrder(TreeNode t1,TreeNode t2){
        if(t2==null) return;
        if(t1.left!=null && t2.left!=null) t1.left.val += t2.left.val;
        else if(t1.left==null && t2.left!=null) {
            t1.left = new TreeNode(t2.left.val);
        }
        if(t1.right!=null && t2.right!=null) t1.right.val += t2.right.val;
        else if(t1.right==null && t2.right!=null) {
            t1.right = new TreeNode(t2.right.val);
        }
        preOrder(t1.left,t2.left);
        preOrder(t1.right,t2.right);
    }
}

class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if(root1==null) return root2;
        if(root2==null) return root1;
        root1.val += root2.val;
        // System.out.println(root1.val+"--"+root2.val);
        root1.left = mergeTrees(root1.left,root2.left);
        root1.right = mergeTrees(root1.right,root2.right);
        return root1;
    }
}

class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if(root1==null) return root2;
        if(root2==null) return root1;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root1);
        q.offer(root2);
        while(!q.isEmpty()){
            TreeNode t1 = q.poll();
            TreeNode t2 = q.poll();
            t1.val += t2.val;
            if(t1.left!=null && t2.left!=null){
                System.out.println(t1.val+"--"+t2.val+"--left");
                q.offer(t1.left);
                q.offer(t2.left);
            }
            if(t1.right!=null && t2.right!=null){
                System.out.println(t1.val+"--"+t2.val+"--right");
                q.offer(t1.right);
                q.offer(t2.right);
            }
            if(t1.left==null && t2.left!=null){
                t1.left = t2.left;
            }
            if(t1.right==null && t2.right!=null){
                t1.right = t2.right;
            }
        }
        return root1;
    }
}
```

## [700. 二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

```java
//递归
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if(root==null) return null;
        else if(root.val==val) return root;
        else if(root.val<val) return searchBST(root.right,val);
        else return searchBST(root.left,val);
    }
}

//迭代
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        while(root!=null){
            if(root.val==val) return root;
            else if(root.val<val) root = root.right;
            else root = root.left;
        }
        return null;
    }
}
```

## [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if(root==null) return true;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode t = q.poll();
            if(leftOrder(t.left,t)==false || rightOrder(t.right,t)==false) return false;
            if(t.left!=null) q.offer(t.left);
            if(t.right!=null) q.offer(t.right);
        }
        return true;
    }

    public boolean leftOrder(TreeNode cur,TreeNode root){
        if(cur==null) return true;
        return root.val>cur.val && leftOrder(cur.left,root) && leftOrder(cur.right,root);
    }

    public boolean rightOrder(TreeNode cur,TreeNode root){
        if(cur==null) return true;
        return root.val<cur.val && rightOrder(cur.left,root) && rightOrder(cur.right,root);
    }
}

class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean isValidBST(TreeNode node, long lower, long upper) {
        if (node == null) {
            return true;
        }
        if (node.val <= lower || node.val >= upper) {
            return false;
        }
        return isValidBST(node.left, lower, node.val) && isValidBST(node.right, node.val, upper);
    }
}

//二叉搜索树的中序遍历结果是升序的
class Solution {
    long maxV = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if(root==null) return true;
        boolean left = isValidBST(root.left);
        if(maxV<root.val) maxV = root.val;
        else return false;
        boolean right = isValidBST(root.right);
        return left && right;
    }
}

class Solution {
    long maxV = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if(root==null) return true;
        Deque<TreeNode> stack = new LinkedList<>();
        while(!stack.isEmpty() || root!=null){
            while(root!=null){
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if(maxV<root.val) maxV = root.val;
            else return false;
            root = root.right;
        }
        return true;
    }
}
```

## [530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)

```java
class Solution {
    int ans = Integer.MAX_VALUE,maxV = -1;
    public int getMinimumDifference(TreeNode root) { 
        inOrder(root);
        return ans;
    }

    public void inOrder(TreeNode root){
        if(root==null) return;
        inOrder(root.left);
        // System.out.println("val:"+root.val);
        if(maxV!=-1 && root.val-maxV < ans) {
            ans = root.val-maxV;
            // System.out.println(ans+"--"+maxV);
        }
        maxV = root.val;
        inOrder(root.right);
    }
}

class Solution {
    int m = Integer.MAX_VALUE;
    TreeNode pre = null;
    public int getMinimumDifference(TreeNode root) {
        if(root==null) return -1;
        getMinimumDifference(root.left);
        if(pre!=null){
            if(m > Math.abs(root.val-pre.val)){
                m = Math.abs(root.val-pre.val);
            }
        }
        pre = root;
        getMinimumDifference(root.right);
        return m;
    }
}

class Solution {
    
    public int getMinimumDifference(TreeNode root) { 
        int ans = Integer.MAX_VALUE,maxV = -1;
        Deque<TreeNode> stack = new LinkedList<>();
        while(!stack.isEmpty() || root!=null){
            while(root!=null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if(maxV!=-1 && root.val-maxV < ans) {
                ans = root.val-maxV;
                // System.out.println(ans+"--"+maxV);
            }
            maxV = root.val;
            root = root.right;
        }
        return ans;
    }
}
```

## [501. 二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)

```java
class Solution {
    List<Integer> ans = new ArrayList<>();
    Map<Integer,Integer> map = new HashMap<>();
    int m = 0;
    public int[] findMode(TreeNode root) {
        inOrder(root);
        // System.out.println(ans.toArray(new Integer[ans.size()]));
        int[] t = new int[ans.size()];
        for(int i=0;i<ans.size();i++) t[i] = ans.get(i);
        return t;
        // return new int[2];
    }

    public void inOrder(TreeNode root){
        if(root==null) return;
        inOrder(root.left);
        map.put(root.val,map.getOrDefault(root.val,0)+1);
        if(map.get(root.val)>m){
            ans.clear();
            ans.add(root.val);
            m = map.get(root.val);
        }
        else if(map.get(root.val)==m){
            ans.add(root.val);
        }
        inOrder(root.right);
    }
}

class Solution {
    Map<Integer,Integer> map = new HashMap<>();
    int m = 0;
    public int[] findMode(TreeNode root) {
        inOrder(root);
        int[] t = new int[map.size()];
        int i = 0;
        for(Integer k:map.keySet()){
            if(map.get(k)==m) t[i++] = k;
        }
        return Arrays.copyOfRange(t,0,i);
    }

    public void inOrder(TreeNode root){
        if(root==null) return;
        inOrder(root.left);
        map.put(root.val,map.getOrDefault(root.val,0)+1);
        if(map.get(root.val)>m){
            m = map.get(root.val);
        }
        inOrder(root.right);
    }
}

class Solution {
    List<Integer> list = new ArrayList<>();
    TreeNode pre = null;
    int cnt = 0,m = -1;
    public int[] findMode(TreeNode root) {
        inOrder(root);
        int[] ans = new int[list.size()];
        for(int i=0;i<list.size();i++){
            ans[i] = list.get(i);
        }
        return ans;
    }
    public void inOrder(TreeNode root){
        if(root==null) return;
        inOrder(root.left);
        if(pre==null){
            cnt = 1;
        }
        else if(pre.val==root.val){
            cnt++;
        }
        else{
            cnt = 1;
        }
        pre = root;
        if(cnt>m){
            m = cnt;
            list.clear();
            list.add(root.val);
        }
        else if(cnt==m){
            list.add(root.val);
        }
        inOrder(root.right);
    }
}

class Solution {
    List<Integer> ans = new ArrayList<>();
    int cnt = 1,maxC = -1,x = -1;
    public int[] findMode(TreeNode root) {
        inOrder(root);
        int[] t = new int[ans.size()];
        for(int i=0;i<ans.size();i++){
            t[i] = ans.get(i);
        }
        return t;
    }

    public void inOrder(TreeNode root){
        if(root==null) return;
        inOrder(root.left);
        update(root.val);
        inOrder(root.right);
    }

    public void update(int val){
        if(val==x){
            cnt++;
        }
        else{
            cnt = 1;
            x = val;
        }
        if(cnt>maxC){
            maxC = cnt;
            ans.clear();
            // System.out.println(cnt+"--"+val);
            ans.add(val);
        }
        else if(cnt==maxC){
            ans.add(val);
            // System.out.println(cnt+"--"+val);
        }
    }
}
```

## [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    boolean f1 = false,f2 = false;
    TreeNode ans;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        postOrder(root,p.val,q.val);
        return ans;
    }
    public void postOrder(TreeNode root,int v1,int v2){
        if(root==null) return;
        postOrder(root.left,v1,v2);
        postOrder(root.right,v1,v2);
        f1 = false;
        f2 = false;
        if(ans==null)
        {
            show(root,v1,v2);
            if(f1==true && f2==true){
                ans = root;
                // System.out.println(ans.val);
            }
        }
    }

    public void show(TreeNode root,int v1,int v2){
        if(root==null) return ;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode t = queue.poll();
            if(t.val==v1) f1 = true;
            else if(t.val==v2) f2 = true;
            if(f1 && f2) return;
            if(t.left!=null) queue.offer(t.left);
            if(t.right!=null)  queue.offer(t.right);
        }
    }
}


class Solution {
    boolean f1 = false,f2 = false;
    TreeNode ans;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null) return null;
        lowestCommonAncestor(root.left,p,q);
        lowestCommonAncestor(root.right,p,q);
        f1 = false;
        f2 = false;
        if(ans==null){
            show(root,p.val,q.val);
            if(f1==true && f2==true){
                ans = root;
                // System.out.println(ans.val);
            }
        }
        return ans;
    }

    public void show(TreeNode root,int v1,int v2){
        if(root==null) return ;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode t = queue.poll();
            if(t.val==v1) f1 = true;
            else if(t.val==v2) f2 = true;
            if(f1 && f2) return;
            if(t.left!=null) queue.offer(t.left);
            if(t.right!=null)  queue.offer(t.right);
        }
    }
}

//找到某条符合条件的边立刻返回
class Solution {
    TreeNode ans;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == q || root == p || root == null) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) return root;
        if (left == null && right != null) return right;
        else if (left != null && right == null) return left;
        else  { //  (left == NULL && right == NULL)
            return null;
        }
    }
}

//给出的是节点的值
public class Solution {
    public int lowestCommonAncestor (TreeNode root, int o1, int o2) {
        // write code here
        if(root==null) return -1;
        if(root.val==o1 || root.val==o2 ) return root.val;
        int left = lowestCommonAncestor(root.left,o1,o2);
        int right = lowestCommonAncestor(root.right,o1,o2);
        if(left!=-1 && right!=-1) return root.val;
        else if(left!=-1 && right==-1) return left;
        else if(right!=-1 && left==-1) return right;
        else return -1;
    }
}
```

# 5.二叉树05

```

```

## [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode ans = root;
        while(true){
            if(ans.val>p.val && ans.val>q.val) ans = ans.left;
            else if(ans.val<p.val && ans.val<q.val) ans = ans.right;
            else break;
        }
        return ans;
    }
}

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null) return null;
        if(root.val>p.val && root.val>q.val){
            TreeNode left = lowestCommonAncestor(root.left,p,q);
            if(left!=null) return left;
        }
        if(root.val<p.val && root.val<q.val){
            TreeNode right = lowestCommonAncestor(root.right,p,q);
            if(right!=null) return right;
        }
        return root;
    }
}

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        List<TreeNode> list1 = getPath(root,p);
        List<TreeNode> list2 = getPath(root,q);
        TreeNode ans = null;
        for(int i=0;i<list1.size() && i<list2.size();i++){
            if(list1.get(i)==list2.get(i)) ans = list1.get(i);
            else break;
        }
        return ans;
    }

    public List<TreeNode> getPath(TreeNode root,TreeNode x){
        List<TreeNode> temp = new ArrayList<>();
        while(true){
            temp.add(root);
            if(root.val>x.val) root = root.left;
            else if(root.val<x.val) root = root.right;
            else break;
        }
        return temp;
    }
}
```

## [701. 二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root==null) return new TreeNode(val);
        TreeNode ans = root;
        while(true){
            if(root.val>val){
                if(root.left!=null) root = root.left;
                else{
                    root.left = new TreeNode(val);
                    break;
                }
            }
            else if(root.val<val){
                if(root.right!=null) root = root.right;
                else{
                    root.right = new TreeNode(val);
                    break;
                }
            }
        }
        return ans;
    }
}

class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root==null) return new TreeNode(val);
        if(root.val<val) root.right = insertIntoBST(root.right,val);
        if(root.val>val) root.left = insertIntoBST(root.left,val);
        return root;
    }
}
```

## [450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode deleteNode(TreeNode root, int x) {
        if(root==null) return null;
        if(root.val==x){
            if(root.left==null && root.right==null){
                root = null;
            }
            else if(root.left!=null){
                TreeNode pre = findPre(root.left);
                root.val = pre.val;
                // System.out.println("pre:"+pre.val);
                root.left = deleteNode(root.left,pre.val);
            }
            else if(root.right!=null){
                TreeNode post = findPost(root.right);
                root.val = post.val;
                // System.out.println("post:"+post.val);
                root.right = deleteNode(root.right,post.val);
            }
        }
        else if(root.val<x) root.right = deleteNode(root.right,x);
        else root.left = deleteNode(root.left,x);
        return root;
    }
    public TreeNode findPre(TreeNode cur){
        while(cur.right!=null) cur = cur.right;
        return cur;
    }

    public TreeNode findPost(TreeNode cur){
        while(cur.left!=null) cur = cur.left;
        return cur;
    }
}

class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root==null){
            return null;
        }
        else if(root.val==key){
            if(root.left==null && root.right==null){
                return null;
            }
            else if(root.left==null){
                return root.right;
            }
            else if(root.right==null){
                return root.left;
            }
            else{
                TreeNode t = root.right;
                while(t.left!=null){
                    t = t.left;
                }
                t.left = root.left;
                return root.right;
            }
        }
        else if(root.val>key){
            root.left = deleteNode(root.left,key);
        }
        else{
            root.right = deleteNode(root.right,key);
        }
        return root;
    }
}
```

## [669. 修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)

```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) {
            return null;
        }
        if (root.val < low) {
            //因为是二叉搜索树,节点.left < 节点 < 节点.right
            //节点数字比low小,就把左节点全部裁掉.
            root = root.right;
            //裁掉之后,继续看右节点的剪裁情况.剪裁后重新赋值给root.
            root = trimBST(root, low, high);
        } else if (root.val > high) {
            //如果数字比high大,就把右节点全部裁掉.
            root = root.left;
            //裁掉之后,继续看左节点的剪裁情况
            root = trimBST(root, low, high);
        } else {
            //如果数字在区间内,就去裁剪左右子节点.
            root.left = trimBST(root.left, low, high);
            root.right = trimBST(root.right, low, high);
        }
        return root;
    }
}
```

## [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

```java
//「本质就是寻找分割点，分割点作为当前节点，然后递归左区间和右区间」。
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        if(nums.length<=0) return null;
        TreeNode root = new TreeNode(nums[nums.length/2]);
        root.left = sortedArrayToBST(Arrays.copyOfRange(nums,0,nums.length/2));
        root.right = sortedArrayToBST(Arrays.copyOfRange(nums,nums.length/2+1,nums.length));
        return root;
    }
}

class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return fun(nums,0,nums.length-1);
    }
    public TreeNode fun(int[] nums,int l,int r){
        if(l>r) return null;
        int mid = (l+r)/2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = fun(nums,l,mid-1);
        root.right = fun(nums,mid+1,r);
        return root;
    }
}

class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        if(nums.length==0) return null;
        Queue<TreeNode> q = new LinkedList<>();
        Queue<Integer> left = new LinkedList<>();
        Queue<Integer> right = new LinkedList<>();
        TreeNode root = new TreeNode(0);
        q.offer(root);
        left.offer(0);
        right.offer(nums.length-1);
        while(!q.isEmpty()){
            TreeNode t = q.poll();
            int l = left.poll();
            int r = right.poll();
            int mid = (l+r)/2;
            t.val = nums[mid];
            if(l<=mid-1){
                left.offer(l);
                right.offer(mid-1);
                t.left = new TreeNode(0);
                q.offer(t.left);
            }
            if(r>=mid+1){
                left.offer(mid+1);
                right.offer(r);
                t.right = new TreeNode(0);
                q.offer(t.right);
            }
        }
        return root;
    }
}
```

## [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

```
class Solution {
    int s = 0;
    public TreeNode convertBST(TreeNode root) {
        if(root==null) return null;
        convertBST(root.right);
        s += root.val;
        root.val = s;
        convertBST(root.left);
        return root;
    }
}
```



## [1373. 二叉搜索子树的最大键值和](https://leetcode-cn.com/problems/maximum-sum-bst-in-binary-tree/)

```java

class Solution {
    int maxV = Integer.MIN_VALUE;
    int s = 0,m = 0;
    public int maxSumBST(TreeNode root) {
        inOrder(root);
        return m;
    }

    public void inOrder(TreeNode root){
        if(root==null) return;
        inOrder(root.left);
        inOrder(root.right);
        s = 0;
        maxV = Integer.MIN_VALUE;
        if(root.left==null && root.right==null){
            if(root.val>m) m = root.val;
        }
        else if(isTrue(root)){
            if(s>m) m = s;
        }
    }
    public boolean isTrue(TreeNode root){
        if(root==null) return true;
        boolean left = isTrue(root.left);
        if(maxV<root.val) maxV = root.val;
        else return false;
        s += root.val;
        boolean right = isTrue(root.right);
        return left && right;
    }
}

class Solution {
    //此题不用考虑0和负数，所以这里设为0
    int maxSum = 0;
    public int maxSumBST(TreeNode root) {
        traverse(root);
        return maxSum;
    }

    int[] traverse(TreeNode root) {
        // base case
        if (root == null) {
            return new int[] {
                1, Integer.MAX_VALUE, Integer.MIN_VALUE, 0
            };
        }

        // 递归计算左右子树
        int[] left = traverse(root.left);
        int[] right = traverse(root.right);

        /******* 后序遍历位置 *******/
        int[] res = new int[4];
        // 这个 if 在判断以 root 为根的二叉树是不是 BST
        if (left[0] == 1 && right[0] == 1 &&
            root.val > left[2] && root.val < right[1]) {
            // 以 root 为根的二叉树是 BST
            res[0] = 1;
            // 计算以 root 为根的这棵 BST 的最小值
            res[1] = Math.min(left[1], root.val);
            // 计算以 root 为根的这棵 BST 的最大值
            res[2] = Math.max(right[2], root.val);
            // 计算以 root 为根的这棵 BST 所有节点之和
            res[3] = left[3] + right[3] + root.val;
            // 更新全局变量
            maxSum = Math.max(maxSum, res[3]);
        } else {
            // 以 root 为根的二叉树不是 BST
            res[0] = 0;
            // 其他的值都没必要计算了，因为用不到
        }
        /**************************/

        return res;
    }
}
```

## 前序\后序和中序构建二叉树

```java
//前序和中序
import java.util.Arrays;
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        if(pre.length==0) return null;
        int preL = 0,preR = pre.length-1;
        int inL = 0,inR = in.length-1; 
        TreeNode root = new TreeNode(pre[preL]);
        int k = 0;
        for(k = inL;k<inR;k++){
            if(in[k]==pre[preL]) break;
        }
        int num = k-inL;
        root.left = reConstructBinaryTree(Arrays.copyOfRange(pre,preL+1,preL+num+1),Arrays.copyOfRange(in,inL,k));
        root.right = reConstructBinaryTree(Arrays.copyOfRange(pre,preL+num+1,preR+1),Arrays.copyOfRange(in,k+1,inR+1));
        return root;
    }
}


//后序和中序
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(postorder.length<=0 || inorder.length<=0) return null;
        int inL = 0,inR = inorder.length;
        int postL = 0,postR = postorder.length;
        TreeNode root = new TreeNode(postorder[postR-1]);
        int k = inL;
        for(;k<inR;k++){
            if(inorder[k]==postorder[postR-1]){
                break;
            }
        }
        int num = k-inL;
        root.left = buildTree(Arrays.copyOfRange(inorder,inL,k),Arrays.copyOfRange(postorder,postL,postL+num));
        root.right = buildTree(Arrays.copyOfRange(inorder,k+1,inR),Arrays.copyOfRange(postorder,postL+num,postR-1));
        return root;
    }
}


class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        return getRoot(0,inorder.length-1,0,postorder.length-1,inorder,postorder);
    }

    public TreeNode getRoot(int inL,int inR,int postL,int postR,int[] inorder, int[] postorder){
        if(postL>postR) return null;
        TreeNode root = new TreeNode(postorder[postR]);
        int k = inL;
        for(;k<=inR;k++){
            if(inorder[k]==postorder[postR]){
                break;
            }
        }
        int num = k-inL;
        root.left = getRoot(inL,k-1,postL,postL+num-1,inorder,postorder);
        root.right = getRoot(k+1,inR,postL+num,postR-1,inorder,postorder);
        return root;
    }
}
```

## 根据层序和中序确定二叉树

```java
package pck01;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class levelAndIn {
    static int n;
    static List<Integer> level = new ArrayList<>();
    static List<Integer> inList = new ArrayList<>();
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        for(int i=0;i<n;i++){
            int x = sc.nextInt();
            level.add(x);
        }
        for(int i=0;i<n;i++){
            int x = sc.nextInt();
            inList.add(x);
        }
        TreeNode root = createLevelAndInTree(level,0,n-1);
        preOrder(root);
    }
/*
7
3 5 4 2 6 7 1
2 5 3 6 4 7 1
 */
    private static void preOrder(TreeNode root) {
        if(root==null) return;

        preOrder(root.left);
        preOrder(root.right);
        System.out.println(root.val);
    }

    private static TreeNode createLevelAndInTree(List<Integer> level, int inL, int inR) {
        if(level.size()==0) return null;
        TreeNode root = new TreeNode(level.get(0));
        int k;
        for(k=inL;k<=inR;k++){
            if(level.get(0).equals(inList.get(k))) break;
        }
        List<Integer> leftLevel = new ArrayList<>();
        List<Integer> rightLevel = new ArrayList<>();
        for(int i=1;i<level.size();i++){
            boolean flag = false;
            for(int j=inL;j<k;j++){
                if(level.get(i)==inList.get(j)){
                    flag = true;
                    break;
                }
            }
            if(flag){
                leftLevel.add(level.get(i));
            }
            else{
                rightLevel.add(level.get(i));
            }
        }
        root.left = createLevelAndInTree(leftLevel,inL,k-1);
        root.right = createLevelAndInTree(rightLevel,k+1,inR);
        return root;
    }

    private static class TreeNode{
        int val;
        TreeNode left,right;
        public TreeNode(){

        }
        public TreeNode(int val){
            this.val = val;
        }
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200723155023885.png)

## 根据层序序列构建二叉树

```java
public static  TreeNode createLevelTree(TreeNode root,String[] s,int len){
    if(len==0) return null;
    else{
        Queue<TreeNode> q = new LinkedList<>();
        int i = 0;
        root = new TreeNode();
        root.val = Integer.parseInt(s[i++]);
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode t = q.poll();
            if(i<len){
                if(!s[i].equals("null")){
                    t.left = new TreeNode();
                    t.left.val = Integer.parseInt(s[i]);
                    q.offer(t.left);
                }
                i++;
            }
            if(i<len){
                if(!s[i].equals("null")){
                    t.right = new TreeNode();
                    t.right.val = Integer.parseInt(s[i]);
                    q.offer(t.right);
                }
                i++;
            }
        }
    }
    return root;
}

static class TreeNode{
    int val;
    TreeNode left,right;
    public TreeNode(){}
    public TreeNode(int val){
        this.val = val;
    }
}
```

## 遍历二叉树满足特定条件就返回

```java
package pck01;

import pck04.Main;

import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;
import java.math.*;
public class test3 {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String[] s = {"3","9","20","6","8","15","7"};
        TreeNode root = createLevelTree(null,s,s.length);
//        preOrder(root);
//        preOrder2(root,0);
        preOrder3(root,root.val);
    }
    private static boolean preOrder2(TreeNode root,int s){
        if(root==null) return false;
        s += root.val;
        System.out.println(root.val);
        //要想回溯的话，s只能定义在函数参数里面，定义在外面，s就会一直加
        if(s==38) return true;
//        System.out.println("left:"+s);
        if (preOrder2(root.left,s)) return true;
        if (preOrder2(root.right,s)) return true;
//        System.out.println("right:"+s);
        return false;
    }
    private static boolean preOrder3(TreeNode root,int s){
        if(root==null) return false;
        System.out.println(root.val);
        if(s==20) return true;
        if(root.left!=null){
            if(preOrder3(root.left,s+root.left.val)) return true;
        }
        if(root.right!=null){
            if(preOrder3(root.right, s + root.right.val)) return true;
        }
        return false;
    }
    private static void preOrder(TreeNode root) {
        if(root==null) return;
        preOrder(root.left);
        preOrder(root.right);
        System.out.println(root.val);
    }

    public static  TreeNode createLevelTree(TreeNode root,String[] s,int len){
        if(len==0) return null;
        else{
            Queue<TreeNode> q = new LinkedList<>();
            int i = 0;
            root = new TreeNode();
            root.val = Integer.parseInt(s[i++]);
            q.offer(root);
            while(!q.isEmpty()){
                TreeNode t = q.poll();
                if(i<len){
                    if(!s[i].equals("null")){
                        t.left = new TreeNode();
                        t.left.val = Integer.parseInt(s[i]);
                        q.offer(t.left);
                    }
                    i++;
                }
                if(i<len){
                    if(!s[i].equals("null")){
                        t.right = new TreeNode();
                        t.right.val = Integer.parseInt(s[i]);
                        q.offer(t.right);
                    }
                    i++;
                }
            }
        }
        return root;
    }

    static class TreeNode{
        int val;
        TreeNode left,right;
        public TreeNode(){}
        public TreeNode(int val){
            this.val = val;
        }
    }
}
```

# 回溯01

## [77. 组合](https://leetcode-cn.com/problems/combinations/)

```java
class Solution {
    LinkedList<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n,k,1);
        return ans;
    }
    public void backtracking(int n,int k,int start){
        if(list.size()==k){
            ans.add(new LinkedList<Integer>(list));
            return;
        }
        for(int i=start;i<=n;i++){
            list.add(i);
            backtracking(n,k,i+1);
            list.removeLast();
        }
    }
}

//剪枝后
class Solution {
    LinkedList<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n,k,1);
        return ans;
    }
    public void backtracking(int n,int k,int start){
        if(list.size()==k){
            ans.add(new LinkedList<Integer>(list));
            return;
        }
        for(int i=start;i<=n-(k-list.size())+1;i++){
            list.add(i);
            backtracking(n,k,i+1);
            list.removeLast();
        }
    }
}
```

## [216. 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)

```java
class Solution {
    LinkedList<List<Integer>> ans = new LinkedList<>();
    LinkedList list = new LinkedList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracking(k,n,1,0);
        return ans;
    }
    public void backtracking(int k,int n,int start,int sum){
        if(sum>n) return;
        if(list.size()==k){
            if(sum==n)
            {
                ans.add(new LinkedList<>(list));
            }
            return;
        }
        for(int i=start;i<=9-(k-list.size())+1;i++){
            list.add(i);
            sum += i;
            backtracking(k,n,i+1,sum);
            list.removeLast();
            sum -= i;
        }
    }
}
```

## [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

```java
class Solution {
    List<String> ans = new LinkedList<>();
    StringBuffer sb = new StringBuffer();
    String[] map = {
        "", // 0
        "", // 1
        "abc", // 2
        "def", // 3
        "ghi", // 4
        "jkl", // 5
        "mno", // 6
        "pqrs", // 7
        "tuv", // 8
        "wxyz", // 9
    };
    public List<String> letterCombinations(String digits) {
        if(digits==null || digits.length()==0) return ans;
        backtracking(digits,0);
        return ans;
    }

    public void backtracking(String digits,int index){
        if(index==digits.length()){
            ans.add(sb.toString());
            return;
        }
        String temp = map[digits.charAt(index)-'0'];
        for(int i=0;i<temp.length();i++){
            sb.append(temp.charAt(i));
            backtracking(digits,index+1);
            sb.deleteCharAt(sb.length()-1);
        }
    }
}
```

# 回溯02

## [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

```java
class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        backtracking(candidates,target,0,0);
        return ans;
    }

    public void backtracking(int[] data,int target,int sum,int index){
        if(sum>target) return;
        if(sum==target){
            ans.add(new LinkedList<>(list));
            return;
        }
        for(int i=index;i<data.length;i++){
            sum += data[i];
            list.add(data[i]);
            backtracking(data,target,sum,i);
            sum -= data[i];
            list.removeLast();
        }
    }
}

//排序后
class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtracking(candidates,target,0,0);
        return ans;
    }

    public void backtracking(int[] data,int target,int sum,int index){
        if(sum>target) return;
        if(sum==target){
            ans.add(new LinkedList<>(list));
            return;
        }
        for(int i=index;i<data.length && sum+data[i]<=target;i++){
            sum += data[i];
            list.add(data[i]);
            backtracking(data,target,sum,i);
            sum -= data[i];
            list.removeLast();
        }
    }
}
```

## [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

```java
class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    int[] used;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        used = new int[candidates.length];
        Arrays.sort(candidates);
        backtracking(candidates,target,0,0);
        return ans;
    }

    public void backtracking(int[] candidates,int target,int sum,int index){
        if(sum>target) return;
        if(sum==target){
            ans.add(new LinkedList<>(list));
            return;
        }
        for(int i=index;i<candidates.length && sum+candidates[i]<=target;i++){
            if(i>0 && candidates[i]==candidates[i-1] && used[i-1]==0) continue;
            used[i] = 1;
            sum += candidates[i];
            list.add(candidates[i]);
            backtracking(candidates,target,sum,i+1);
            used[i] = 0;
            sum -= candidates[i];
            list.removeLast();
        }
    }
}
```

## [131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

```java
class Solution {
    List<List<String>> ans = new LinkedList<>();
    LinkedList<String> list = new LinkedList<>();
    public List<List<String>> partition(String s) {
        backtracking(s,0);
        return ans;
    }

    public void backtracking(String s,int index){
        if(index>=s.length()){
            ans.add(new LinkedList<>(list));
            return;
        }
        for(int i=index;i<s.length();i++){
            if(judge(s,index,i)){
                list.add(s.substring(index,i+1));
            }
            else{
                continue;
            }
            backtracking(s,i+1);
            list.removeLast();
        }
    }

    public boolean judge(String s,int l,int r){
        for(int i=l,j=r;i<=j;i++,j--){
            if(s.charAt(i)!=s.charAt(j)) return false;
        }
        return true;
    }
} 
```

## [93. 复原 IP 地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

```java
class Solution {
    List<String> ans = new LinkedList<>();

    public List<String> restoreIpAddresses(String s) {
        StringBuffer sb = new StringBuffer(s);
        backtracking(sb,0,0);
        return ans;
    }

    public void backtracking(StringBuffer s,int startIndex,int cnt){
        if(cnt==3){
            if(isValid(s,startIndex,s.length()-1)){
                ans.add(s.toString());
            }
            return;
        }
        for(int i=startIndex;i<s.length();i++){
            if(isValid(s,startIndex,i)){
                s.insert(i+1,'.');
                cnt++;
                backtracking(s,i+2,cnt);
                cnt--;
                s.deleteCharAt(i+1);
            }
            else{
                break;
            }
        }
    }

    private boolean isValid(StringBuffer s, int start, int end) {
        if(start>end){
            return false;
        }
        if(s.charAt(start)=='0' && start!=end){
            return false;
        }
        int num = 0;
        for(int i=start;i<=end;i++){
            num = num*10+s.charAt(i)-'0';
            if(num>255) return false;
        }
        return true;
    }
}

class Solution {
    List<String> ans = new ArrayList<>();
    LinkedList<String> temp = new LinkedList<>();
    public List<String> restoreIpAddresses(String s) {
        backtracking(s,0,0,s.length());
        return ans;
    }
    public void backtracking(String s,int index,int num,int n){
        if(num==4){
            StringBuilder sb = new StringBuilder();
            System.out.println(temp);
            for(int i=0;i<4;i++){
                if(i!=0) sb.append(".");
                sb.append(temp.get(i));
            }
            ans.add(sb.toString());
            return;
        }
        for(int i=index;i<n;i++){
            if(num==3){
                i = n-1;
            }
            // System.out.println("index:"+index+"--i:"+i);
            if(judge(index,i,s)){
                num++;
                temp.add(s.substring(index,i+1));
            }
            else{
                return;
            }
            backtracking(s,i+1,num,n);
            temp.removeLast();
            num--;
        }
    }

    public boolean judge(int start,int end,String s){
        if(end-start>=3) return false;
        if(s.charAt(start)=='0'&&start!=end) return false;
        int m = 0;
        for(int i=start;i<=end;i++){
            m = m*10 + s.charAt(i)-'0';
            if(m>255) return false;
        }
        // System.out.println("m:"+m);
        return true;
    }
}
```

## [78. 子集](https://leetcode-cn.com/problems/subsets/)

```java
class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    public List<List<Integer>> subsets(int[] nums) {
        backtracking(nums,0);
        ans.add(new LinkedList<>());
        return ans;
    }

    public void backtracking(int[] nums,int startIndex){
        for(int i=startIndex;i<nums.length;i++){
            list.add(nums[i]);
            ans.add(new LinkedList<>(list));
            backtracking(nums,i+1);
            list.removeLast();
        }
    }
}
```

# 回溯03

## [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)

```java
class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    int[] vis;
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        vis = new int[nums.length];
        Arrays.sort(nums);
        backtracking(nums,0);
        ans.add(new LinkedList<>());
        return ans;
    }

    public void backtracking(int[] nums,int startIndex){
        for(int i=startIndex;i<nums.length;i++){
            if(i>0 && nums[i]==nums[i-1] && vis[i-1]==0) continue;
            vis[i] = 1;
            list.add(nums[i]);
            ans.add(new LinkedList<>(list));
            backtracking(nums,i+1);
            vis[i] = 0;
            list.removeLast();
        }
    }
}

class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        backtracking(nums,0);
        ans.add(new LinkedList<>());
        return ans;
    }

    public void backtracking(int[] nums,int startIndex){
        for(int i=startIndex;i<nums.length;i++){
            list.add(nums[i]);
            if(!ans.contains(list))
                ans.add(new LinkedList<>(list));
            backtracking(nums,i+1);
            list.removeLast();
        }
    }
}

class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    // int[] vis;
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        // vis = new int[nums.length];
        Arrays.sort(nums);
        backtracking(nums,0);
        ans.add(new LinkedList<>());
        return ans;
    }

    public void backtracking(int[] nums,int startIndex){
        TreeSet<Integer> set = new TreeSet<>();
        for(int i=startIndex;i<nums.length;i++){
            //去重切记要排序，排完序，用集合/数组的效果是一样的
            // if(i>0 && nums[i]==nums[i-1] && vis[i-1]==0) continue;
            if(set.contains(nums[i])){
                System.out.println(i+"--"+nums[i]);
                continue;
            }
            // vis[i] = 1;
            set.add(nums[i]);
            list.add(nums[i]);
            ans.add(new LinkedList<>(list));
            backtracking(nums,i+1);
            // vis[i] = 0;
            list.removeLast();
        }
    }
}
```

## [491. 递增子序列](https://leetcode-cn.com/problems/increasing-subsequences/)

```java
//暴力，超时了
class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    int[] vis;
    public List<List<Integer>> findSubsequences(int[] nums) {
        vis = new int[nums.length];
        backtracking(nums,0,0);
        return ans;
    }

    public void backtracking(int[] nums,int startIndex,int cnt){
        if(cnt>=2){
            if(!ans.contains(list))
                ans.add(new LinkedList<>(list));
        }
        for(int i=startIndex;i<nums.length;i++){
            if(i>0 && nums[i]==nums[i-1] && vis[i-1]==0) continue;
            if(list.size()>0 && nums[i]<list.getLast()) continue;
            list.add(nums[i]);
            cnt++;
            vis[i] = 1;
            backtracking(nums,i+1,cnt);
            cnt--;
            vis[i] = 0;
            list.removeLast();
        }
    }
}

class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    int[] vis;
    public List<List<Integer>> findSubsequences(int[] nums) {
        vis = new int[nums.length];
        backtracking(nums,0,0);
        return ans;
    }

    public void backtracking(int[] nums,int startIndex,int cnt){
        if(cnt>=2){
            ans.add(new LinkedList<>(list));
        }
        //Map<Integer,Integer> map = new HashMap<>();也可以
        TreeSet<Integer> set = new TreeSet<>();
        for(int i=startIndex;i<nums.length;i++){
            if(i>0 && nums[i]==nums[i-1] && vis[i-1]==0) continue;
            if(list.size()>0 && nums[i]<list.getLast()) continue;
            if(set.contains(nums[i])) continue;
            set.add(nums[i]);
            list.add(nums[i]);
            cnt++;
            vis[i] = 1;
            backtracking(nums,i+1,cnt);
            cnt--;
            vis[i] = 0;
            list.removeLast();
        }
    }
}

class Solution {
    LinkedList<Integer> temp = new LinkedList<>();
    List<List<Integer>> ans = new ArrayList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        backtracking(nums,nums.length,0);
        return ans;
    }

    public void backtracking(int[] nums,int n,int index){
        if(temp.size()>1){
            ans.add(new LinkedList<>(temp));
        }
        int[] vis = new int[201];
        for(int i=index;i<n;i++){
            if((temp.size()>0 && nums[i]<temp.get(temp.size()-1)) || vis[nums[i]+100]==1){
                continue;
            }
            vis[nums[i]+100] = 1;
            temp.add(nums[i]);
            backtracking(nums,n,i+1);
            temp.removeLast();
        }
    }
}

关于去重问题（都可看做对本层元素的去重）：
    （1）能排序，先排序，在用访问数组或者集合的方式判断是否添加过
    （2）不能排序，用集合处理，但是集合不一定能生效
```

## [46. 全排列](https://leetcode-cn.com/problems/permutations/)

```java
class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    public List<List<Integer>> permute(int[] nums) {
        boolean[] vis = new boolean[nums.length];
        bakctracking(nums,nums.length,vis);
        return ans;
    }

    public void bakctracking(int[] nums,int n,boolean[] vis){
        if(list.size()==n){
            ans.add(new LinkedList<>(list));
            return;
        }
        for(int i=0;i<n;i++){
            if(vis[i]==true) continue;
            vis[i] = true;
            list.add(nums[i]);
            bakctracking(nums,n,vis);
            vis[i] = false;
            list.removeLast();
        }
    }
}
```

## [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

```java
class Solution {
    List<List<Integer>> ans = new LinkedList<>();
    LinkedList<Integer> list = new LinkedList<>();
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        boolean[] vis = new boolean[nums.length];
        backtracking(nums,nums.length,vis);
        return ans;
    }

    public void backtracking(int[] nums,int n,boolean[] vis){
        if(list.size()==n){
            ans.add(new LinkedList<>(list));
            return;
        }
        for(int i=0;i<n;i++){
            if(vis[i]==true) continue;
            if(i>0 && nums[i]==nums[i-1] && vis[i-1]==false){
                continue;
            }
            list.add(nums[i]);
            vis[i] = true;
            backtracking(nums,n,vis);
            vis[i] = false;
            list.removeLast();
        }
    }
}
```

## [332. 重新安排行程](https://leetcode-cn.com/problems/reconstruct-itinerary/)

```java
class Solution {
    LinkedList<String> ans = new LinkedList<>();
    Map<String,LinkedList<String>> map = new HashMap<>();
    public List<String> findItinerary(List<List<String>> tickets) {
        for(List<String> list:tickets){
            LinkedList<String> lt = new LinkedList<>();
            if(map.containsKey(list.get(0))){
                lt = map.get(list.get(0));
                lt.add(list.get(1));
                Collections.sort(lt);
                map.put(list.get(0),lt);
            }
            else{
                lt.add(list.get(1));
                map.put(list.get(0),lt);
            }
        }
        // System.out.println(map);
        ans.add("JFK");
        backtracking(tickets.size()+1);
        return ans;
    }

    public boolean backtracking(int cnt){
        if(ans.size()==cnt){
            return true;
        }
        LinkedList<String> list = map.get(ans.getLast());
        // System.out.println(ans.getLast()+"---"+list);
        if(list!=null){
            for(int i=0;i<list.size();i++){
                String t = list.get(i);
                ans.add(t);
                list.remove(i);
                if(backtracking(cnt)) return true;
                ans.removeLast();
                list.add(i,t);
            }
        } 
        return false;
    }
}

class Solution {
    LinkedList<String> ans = new LinkedList<>();
    Map<String,Map<String,Integer>> map = new TreeMap<>();
    public List<String> findItinerary(List<List<String>> tickets) {
        for(List<String> ticket:tickets){
            if(map.get(ticket.get(0))==null){
                Map<String,Integer> temp = new TreeMap<>();
                temp.put(ticket.get(1),1);
                map.put(ticket.get(0),temp);
            }
            else{
                Map<String,Integer> temp = map.get(ticket.get(0));
                if(temp.get(ticket.get(1))==null){
                    temp.put(ticket.get(1),1);
                }
                else{
                    temp.put(ticket.get(1),temp.get(ticket.get(1))+1);
                }
                // Collections.sort(temp);
                map.put(ticket.get(0),temp);
            }
        }
       
        // System.out.println(map);
        ans.add("JFK");
        // System.out.println(tickets.size());
        backtracking(tickets.size());
        return ans;
    }

    public boolean backtracking(int ticketNum){
        if(ans.size()==ticketNum+1){
            return true;
        }
        // System.out.println("ans:"+ans);
        Map<String,Integer> temp = map.get(ans.get(ans.size()-1));
        // System.out.println("temp:"+temp);
        if(temp!=null){
            for(String key:temp.keySet()){
                int r = temp.get(key);
                if(r>0){
                    r--;
                    temp.put(key,r);
                    // map.put(ans.get(ans.size()-1),temp);
                    ans.add(key);
                    // System.out.println("map:"+map);
                    if(backtracking(ticketNum)) return true;
                    r++;
                    temp.put(key,r);
                    // map.put(ans.get(ans.size()-1),temp);
                    ans.removeLast();
                }
            }
        }
        return false;
    }
}

class Solution {
    LinkedList<String> ans = new LinkedList<>();
    Map<String,Map<String,Integer>> map = new TreeMap<>();
    public List<String> findItinerary(List<List<String>> tickets) {
        Map<String,Integer> temp;
        for(List<String> ticket:tickets){
            if(map.get(ticket.get(0))==null){
                temp = new TreeMap<>();
                temp.put(ticket.get(1),1);     
            }
            else{
                temp = map.get(ticket.get(0));
                if(temp.get(ticket.get(1))==null){
                    temp.put(ticket.get(1),1);
                }
                else{
                    temp.put(ticket.get(1),temp.get(ticket.get(1))+1);
                }
            }
            map.put(ticket.get(0),temp);
        }
       
        // System.out.println(map);
        ans.add("JFK");
        // System.out.println(tickets.size());
        backtracking(tickets.size());
        return ans;
    }

    public boolean backtracking(int ticketNum){
        if(ans.size()==ticketNum+1){
            return true;
        }
        // System.out.println("ans:"+ans);
        String last = ans.getLast();
        if(map.containsKey(last)){
            for(Map.Entry<String,Integer> temp:map.get(last).entrySet()){
                int r = temp.getValue();
                if(r>0){
                    ans.add(temp.getKey());
                    temp.setValue(r-1);
                    if(backtracking(ticketNum)) return true;
                    ans.removeLast();
                    temp.setValue(r);
                }
            }
        }
        return false;
    }
}
```

## [51. N 皇后](https://leetcode-cn.com/problems/n-queens/)

```java
class Solution {
    List<List<String>> ans = new ArrayList<>();
    
    public List<List<String>> solveNQueens(int n) {
        char[][] symbol = new char[n][n];
        for(char[] c:symbol){
            Arrays.fill(c,'.');
        }
        backtracking(n,0,symbol);
        return ans;
    }

    public void backtracking(int n,int row,char[][] symbol){
        if(row>=n){
            List<String> temp = new ArrayList<>();
            for(char[] c:symbol){
                temp.add(String.copyValueOf(c));
               // temp.add(String.valueOf(c));
            }
            ans.add(new ArrayList<>(temp));
            return;
        }
        for(int col=0;col<n;col++){
            if(judge(row,col,symbol,n)){
                symbol[row][col] = 'Q';
                backtracking(n,row+1,symbol);
                symbol[row][col] = '.';
            }
        }
    }

    public boolean judge(int row,int col,char[][] symbol,int n){
        for(int i=0;i<row;i++){
            if(symbol[i][col]=='Q') return false;
        }
        for(int i=row-1,j=col+1;i>=0&&j<n;i--,j++){
            if(symbol[i][j]=='Q') return false;
        }
        for(int i=row-1,j=col-1;i>=0&&j>=0;i--,j--){
            if(symbol[i][j]=='Q') return false;
        }
        return true;
    }
}

class Solution {
    List<List<String>> ans = new LinkedList<>();
    LinkedList<String> list = new LinkedList<>();
    public List<List<String>> solveNQueens(int n) {
        for(int i=0;i<n;i++){
            StringBuilder sb = new StringBuilder();
            for(int j=0;j<n;j++) sb.append('.');
            list.add(sb.toString());
        }
        backtracking(n,0);
        return ans;
    }

    public void backtracking(int n,int row){
        if(row==n){
            ans.add(new LinkedList<>(list));
            return;
        }
        for(int col=0;col<n;col++){
            if(lawful(row,col,n,list)){
                StringBuffer sb = new StringBuffer(list.get(row));
                sb.setCharAt(col,'Q');
                list.set(row,sb.toString());
                backtracking(n,row+1);
                sb.setCharAt(col,'.');
                list.set(row,sb.toString());
            }
        }
    }

    public boolean lawful(int row,int col,int n,LinkedList<String> list){
        for(int i=0;i<row;i++){
            if(list.get(i).charAt(col)=='Q') return false;
        }
        for(int i=row-1,j=col-1;i>=0&&j>=0;i--,j--){
            if(list.get(i).charAt(j)=='Q') return false;
        }
        for(int i=row-1,j=col+1;i>=0&&j<n;i--,j++){
            if(list.get(i).charAt(j)=='Q') return false;
        }
        return true;
    }
}

class Solution {
    List<List<String>> ans = new LinkedList<>();
    LinkedList<String> list = new LinkedList<>();
    public List<List<String>> solveNQueens(int n) {
        backtracking(n,0);
        return ans;
    }

    public void backtracking(int n,int row){
        if(row==n){
            ans.add(new LinkedList<>(list));
            return;
        }
        for(int col=0;col<n;col++){
            if(lawful(row,col,n)){
                list.add(geneString(col,n));
                backtracking(n,row+1);
                list.removeLast();
            }
        }
    }

    public boolean lawful(int row,int col,int n){
        for(int i=0;i<row;i++){
            if(list.get(i).charAt(col)=='Q') return false;
        }
        for(int i=row-1,j=col-1;i>=0&&j>=0;i--,j--){
            if(list.get(i).charAt(j)=='Q') return false;
        }
        for(int i=row-1,j=col+1;i>=0&&j<n;i--,j++){
            if(list.get(i).charAt(j)=='Q') return false;
        }
        return true;
    }
    public String geneString(int col,int n){
        StringBuilder sb = new StringBuilder();
        for(int i=0;i<col;i++) sb.append('.');
        sb.append('Q');
        for(int i=col+1;i<n;i++) sb.append('.');
        return sb.toString();
    }
}
```

## 37. 解数独
```java
class Solution {
    public void solveSudoku(char[][] board) {
        backtracking(board);
    }

    public boolean backtracking(char[][] board){
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                if(board[i][j]!='.') continue;
                for(char k='1';k<='9';k++){
                    if(judge(i,j,k,board)){
                        board[i][j] = k;
                        if(backtracking(board)) return true;
                        board[i][j] = '.';
                    }
                }
                return false;
            }
        }
        return true;
    }

    boolean judge(int row,int col,char c,char[][] board){

        for(int i=0;i<board.length;i++){
            if(board[i][col]==c) return false;
        }

        for(int j=0;j<board[0].length;j++){
            if(board[row][j]==c) return false;
        }

        int startRow = (row/3)*3;
        int startCol = (col/3)*3;
        for(int i=startRow;i<startRow+3;i++){
            for(int j=startCol;j<startCol+3;j++){
                if(board[i][j]==c) return false;
            }
        }

        return true;
    }
}
```
# 贪心

## [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        int ans = 0;
        Arrays.sort(g);
        Arrays.sort(s);
        for(int i=g.length-1;i>=0;i--){
            for(int j=s.length-1;j>=0;j--){
                if(s[j]>=g[i]){
                    ans++;
                    s[j] = 0;
                    break;
                }
            }
        }
        return ans;
    }
}
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        int ans = 0;
        Arrays.sort(g);
        Arrays.sort(s);
        for(int i=g.length-1,j = s.length-1;i>=0&&j>=0;){
            if(s[j]>=g[i]){
                j--;
                i--;
                ans++;
            }
            else{
                i--;
            }
        }
        return ans;
    }
}
```

## [376. 摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int ans = 1;
        int pre = 0,cur = 0;
        for(int i=1;i<nums.length;i++){
            cur = nums[i]-nums[i-1];
            if((cur<0 && pre>=0) || (cur>0 && pre<=0)){
                ans++;
                pre = cur;
            }
        }
        return ans;
    }
}
```

## [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int ans = 0;
        for(int i=1;i<prices.length;i++){
            if(prices[i]-prices[i-1]>0){
                ans += prices[i]-prices[i-1];
            }
        }
        return ans;
    }
}
```

## [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

```java
class Solution {
    public boolean canJump(int[] nums) {
        int cnt = 0;
        for(int i=0;i<nums.length;i++){
            if(i<=cnt){
                cnt = Math.max(cnt,nums[i]+i);
                if(cnt>=nums.length-1) return true;
            }
        }
        return false;
    }
}
class Solution {
    public boolean canJump(int[] nums) {
        int cnt = 0;
        for(int i=0;i<=cnt;i++){
            cnt = Math.max(cnt,nums[i]+i);
            if(cnt>=nums.length-1) return true;
        }
        return false;
    }
}
```

## [45. 跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)

```java
class Solution {
    public int jump(int[] nums) {
        int cnt = 0,ans = 0;
        int start = 0,end = 1;
        while(end<nums.length){
            for(int i=start;i<end;i++){
                cnt = Math.max(cnt,i+nums[i]);
            }
            start = end;
            end = cnt+1;
            ans++;
        }
        return ans;
    }
   
}
```

## [1005. K 次取反后最大化的数组和](https://leetcode-cn.com/problems/maximize-sum-of-array-after-k-negations/)

```java
class Solution {
    public int largestSumAfterKNegations(int[] A, int K) {
        Arrays.sort(A);
        int t = 0,ans = 0;
        for(int i=0;i<A.length;i++){
            if(K>0 && A[i]<0){
                t = i;
                K--;
                A[i] = -A[i];
            }
        }
        if(K>0){
            if(K%2!=0){
                if(t+1<A.length){
                    if(A[t]<A[t+1]) A[t] = -A[t];
                    else A[t+1] = -A[t+1];
                }
                else{
                    A[t] = -A[t];
                }
            }
        }
        for(int i=0;i<A.length;i++){
            ans += A[i];
        }
        return ans;
    }
}

class Solution {
    public int largestSumAfterKNegations(int[] A, int K) {
        Integer[] AA = new Integer[A.length];
        for(int i=0;i<A.length;i++) AA[i] = A[i];
        Arrays.sort(AA,new Comparator<Integer>(){
            public int compare(Integer o1,Integer o2){
                return Math.abs(o2)-Math.abs(o1);
            }
        });
        int t = 0,ans = 0;
        for(int i=0;i<AA.length;i++){
            if(K>0 && AA[i]<0){
                t = i;
                K--;
                AA[i] = -AA[i];
            }
        }
        if(K%2!=0){
           AA[AA.length-1] = -AA[AA.length-1];
        }
        for(int i=0;i<AA.length;i++){
            ans += AA[i];
        }
        return ans;
    }
}
```

## [135. 分发糖果](https://leetcode-cn.com/problems/candy/)

```java
class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] score = new int[n];
        for(int i=0;i<n;i++){
            score[i] = 1;
        }
        for(int i=1;i<n;i++){
            if(ratings[i]>ratings[i-1]) score[i] = score[i-1]+1;
        }
        for(int i=n-2;i>=0;i--){
            if(ratings[i]>ratings[i+1]){
                score[i] = Math.max(score[i],score[i+1]+1);
            }
        }
        int ans = 0;
        for(int i=0;i<n;i++) ans += score[i];
        return ans;
    }
}
```

## [134. 加油站](https://leetcode-cn.com/problems/gas-station/)

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int n = gas.length;
        int left = 0,cur = 0,cnt = 0,ans = -1;
        for(int i=0;i<n;i++){
            int j = i;
            left = 0;
            cnt = 0;
            while(cnt<n){
                cur = left+gas[j];
                if(cur>=cost[j]) {
                    cnt++;
                    left = cur-cost[j];
                }
                else break;
                j++;
                if(j>=n) j = 0;
            }
             if(cnt>=n){
                 ans = i;
                 break;
             }
        }
        return ans;
    }
}

class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum = 0;
        int totalSum = 0;
        int start = 0;
        for (int i = 0; i < gas.size(); i++) {
            curSum += gas[i] - cost[i];
            totalSum += gas[i] - cost[i];
            if (curSum < 0) {   // 当前累加rest[i]和 curSum一旦小于0
                start = i + 1;  // 起始位置更新为i+1
                curSum = 0;     // curSum从0开始
            }
        }
        if (totalSum < 0) return -1; // 说明怎么走都不可能跑一圈了
        return start;
    }
}
```

## [860. 柠檬水找零](https://leetcode-cn.com/problems/lemonade-change/)

```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int n = bills.length;
        int[] left = new int[3];
        for(int i=0;i<n;i++){
            if(bills[i]==5){
                left[0] += 1;
            }
            else if(bills[i]==10){
                if(left[0]>0){
                    left[0] -= 1;
                    left[1] += 1;
                }
                else{
                    return false;
                }
            }
            else{
                if(left[0]>0 && left[1]>0){
                    left[0] -= 1;
                    left[1] -= 1;
                }
                else if(left[0]>=3){
                    left[0] -= 3;
                }
                else{
                    return false;
                }
                left[2] += 1;
            }
        }
        return true;
    }
}
```

## [406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people,new Comparator<int[]>(){
            public int compare(int[] o1,int[] o2){
                if(o1[0]==o2[0]) return o1[1]-o2[1];
                return o2[0]-o1[0];
            }
        });
        List<List<Integer>> list = new LinkedList<>();
        for(int i=0;i<people.length;i++){
            int x = people[i][1];
            if(x>list.size()) x = list.size();
            List<Integer> t = new LinkedList<>();
            t.add(people[i][0]);
            t.add(people[i][1]);
            list.add(x,t);
        }
        for(int i=0;i<list.size();i++){
            people[i][0] = list.get(i).get(0);
            people[i][1] = list.get(i).get(1);
        }
        return people;
    }
}
```

# 动态规划01

## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

```java
class Solution {
    public int climbStairs(int n) {
        if(n<=2) return n;
        int[] d = new int[n+1];
        d[1] = 1;
        d[2] = 2;
        for(int i=3;i<=n;i++){
            d[i] = d[i-1]+d[i-2];
        }
        return d[n];
    }
}

class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=2;j++){
                if(i>=j){
                    dp[i] += dp[i-j];
                }
            }
        }
        return dp[n];
    }
}
```

## [746. 使用最小花费爬楼梯](https://leetcode-cn.com/problems/min-cost-climbing-stairs/)

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int ans = 0;
        int[] d = new int[cost.length];
        d[0] = cost[0];
        d[1] = cost[1];
        int i = 2;
        for(;i<cost.length;i++){
            d[i] = Math.min(d[i-1],d[i-2])+cost[i];
            // System.out.println(d[i]);
        }
        return Math.min(d[i-1],d[i-2]);
    }
}
```

## [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

```java
//dfs，超时了
class Solution {
    public int uniquePaths(int m, int n) {
        return dfs(1,1,m,n);
    }

    public int dfs(int i,int j,int m,int n){
        if(i>m || j>n) return 0;
        if(i==m && j==n) return 1;
        return dfs(i+1,j,m,n) + dfs(i,j+1,m,n);
    }
}

//动态规划
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] d = new int[m][n];
        for(int i=0;i<n;i++) d[0][i] = 1;
        for(int i=0;i<m;i++) d[i][0] = 1;
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++) d[i][j] = d[i-1][j]+d[i][j-1];
        }
        return d[m-1][n-1];
    }
}
```

## [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int n = obstacleGrid.length,m = obstacleGrid[0].length;
        int[][] d = new int[n][m];
        for(int i=0;i<n;i++){
            if(obstacleGrid[i][0]==1){
                break;
            }
            else{
                d[i][0] = 1;
            }
        }
        for(int j=0;j<m;j++){
            if(obstacleGrid[0][j]==1){
                break;
            }
            else{
                d[0][j] = 1;
            }
        }
        for(int i=1;i<n;i++){
            for(int j=1;j<m;j++){
                if(obstacleGrid[i][j]!=1){
                    d[i][j] = d[i-1][j]+d[i][j-1];
                }
            }
        }
        return d[n-1][m-1];
    }
}
```

## [343. 整数拆分](https://leetcode-cn.com/problems/integer-break/)

## [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

```java
class Solution {
    public int numTrees(int n) {
        int[] d = new int[n+1];
        d[0] = 1;
        for(int i=1;i<=n;i++){
            for(int j=0;j<=i-1;j++){
                d[i] += d[j]*d[i-j-1];
            }
        }
        return d[n];
    }
}
```

# 动态规划03

## [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int[] d = new int[20010];
        int s = 0;
        for(int i=0;i<nums.length;i++){
            s += nums[i];
        }
        if(s%2==1) return false;
        s /= 2;
        for(int i=0;i<nums.length;i++){
            for(int j=s;j>=nums[i];j--){
                d[j] = Math.max(d[j],d[j-nums[i]]+nums[i]);
            }
        }
        if(s==d[s]) return true;
        return false;
    }
}
```

## [494. 目标和](https://leetcode-cn.com/problems/target-sum/)

```java
class Solution {
    int cnt = 0,n = 0;
    public int findTargetSumWays(int[] nums, int target) {
        n = nums.length;
        cal(nums,target,0,0);
        return cnt;
    }

    public void cal(int[] nums,int target,int index,int s){
        if(index==n){
            if(s==target) cnt++;
        }
        else{
            cal(nums,target,index+1,s+nums[index]);
            cal(nums,target,index+1,s-nums[index]);
        }
    }
}

class Solution {
    int cnt = 0,n = 0,bag = 0;
    public int findTargetSumWays(int[] nums, int target) {
        Arrays.sort(nums);
        n = nums.length;
        int sum = 0;
        for(int i=0;i<n;i++) sum += nums[i];
        bag = target+sum;
        if(bag%2==1) return 0;
        cal(nums,bag/2,0,0);
        return cnt;
    }

    public void cal(int[] nums,int target,int index,int s){
       if(s==target){
           cnt++;
       }
       for(int i=index;i<n&&nums[i]+s<=target;i++){
           s += nums[i];
           cal(nums,target,i+1,s);
           s -= nums[i];
       }
    }
}

class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int n = 0,bag = 0;
        n = nums.length;
        int sum = 0;
        for(int i=0;i<n;i++) sum += nums[i];
        bag = target+sum;
        if(bag%2==1) return 0;
        bag /= 2;
        int[] dp = new int[bag+10];
        dp[0] = 1;
        for(int i=0;i<n;i++){
            for(int j=bag;j>=nums[i];j--){
                dp[j] += dp[j-nums[i]];
            }
        }
        return dp[bag];
    }
}
```

## [1049. 最后一块石头的重量 II](https://leetcode-cn.com/problems/last-stone-weight-ii/)

```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for(int i=0;i<stones.length;i++) sum += stones[i];
        int tar = sum/2;
        int dp[] = new int[tar+1];
        for(int i=0;i<stones.length;i++){
            for(int j=tar;j>=stones[i];j--){
                dp[j] = Math.max(dp[j],dp[j-stones[i]]+stones[i]);
            }
        }
        return sum - 2*dp[tar];
    }
}
```

## [474. 一和零](https://leetcode-cn.com/problems/ones-and-zeroes/)

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m+1][n+1];
        for(String str:strs){
            int oneNum = 0,zeroNum = 0;
            for(int i=0;i<str.length();i++){
                if(str.charAt(i)=='0') zeroNum++;
                else if(str.charAt(i)=='1') oneNum++;
            }
            for(int i=m;i>=zeroNum;i--){
                for(int j=n;j>=oneNum;j--){
                    dp[i][j] = Math.max(dp[i][j],dp[i-zeroNum][j-oneNum]+1);
                }
            }
        }
        return dp[m][n];
    }
}
```

## [518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

```java
class Solution {
    int cnt = 0;
    public int change(int amount, int[] coins) {
        back(coins,amount,0,0,coins.length);
        return cnt;
    }
    public void back(int[] coins,int amount,int index,int sum,int n){
        if(sum>amount) return;
        if(sum==amount){
            cnt++;
        }
        for(int i=index;i<n&&sum<amount;i++){
            sum += coins[i];
            back(coins,amount,i,sum,n);
            sum -= coins[i];
        }
    }
}

class Solution {
    int cnt = 0;
    public int change(int amount, int[] coins) {
        int n = coins.length;
        int[] dp = new int[amount+1];
        dp[0] = 1;
        for(int i=0;i<n;i++){
            for(int j=coins[i];j<=amount;j++){
                dp[j] += dp[j-coins[i]];
            }
        }
        return dp[amount];
    }
}
```

# 动态规划04

## [377. 组合总和 Ⅳ](https://leetcode-cn.com/problems/combination-sum-iv/)

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int n = nums.length;
        int[] dp = new int[target+1];
        dp[0] = 1;
        for(int i=1;i<=target;i++){
            for(int j=0;j<nums.length;j++){
                if(i>=nums[j]){
                    dp[i] += dp[i-nums[j]];
                }
            }
        }
        return dp[target];
    }
}
```

## [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int n = coins.length;
        int[] dp = new int[amount+1];
        for(int i=1;i<=amount;i++){
            dp[i] = Integer.MAX_VALUE;
        }
        for(int i=0;i<n;i++){
            for(int j=coins[i];j<=amount;j++){
                if(dp[j-coins[i]]!=Integer.MAX_VALUE){
                    dp[j] = Math.min(dp[j],dp[j-coins[i]]+1);
                }
            }
        }
        return dp[amount]==Integer.MAX_VALUE?-1:dp[amount];
    }
}
```

## [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n+1];
        int x = (int) Math.sqrt(Double.valueOf(n));
        if(x*x==n) return 1;
        for(int i=1;i<=n;i++) dp[i] = Integer.MAX_VALUE;
        for(int i=1;i<=x;i++){
            for(int j=i*i;j<=n;j++){
                dp[j] = Math.min(dp[j],dp[j-i*i]+1);
            }
        }
        return dp[n];
    }
}

class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n+1];
        for(int i=1;i<=n;i++) dp[i] = Integer.MAX_VALUE;
        for(int i=1;i<=n;i++){
            for(int j=1;j*j<=i;j++){
                dp[i] = Math.min(dp[i],dp[i-j*j]+1);
            }
        }
        return dp[n];
    }
}
```

# 动态规划05

## [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)

```java

class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        return backtracking(s,wordDict,0);
    }

    public boolean backtracking(String s,List<String> wordDict,int index){
        if(index>=s.length()){
            return true;
        }

        for(int i=index;i<s.length();i++){
            String temp = s.substring(index,i+1);
            if(wordDict.contains(temp) && backtracking(s,wordDict,i+1)){
                return true;
            }
        }
        return false;
    }
}

class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int[] flag = new int[s.length()];
        Arrays.fill(flag,-1);
        return backtracking(s,wordDict,0,flag);
    }

    public boolean backtracking(String s,List<String> wordDict,int index,int[] flag){
        if(index>=s.length()){
            return true;
        }
        if(flag[index]!=-1) return flag[index]==1;
        for(int i=index;i<s.length();i++){
            if(wordDict.contains(s.substring(index,i+1)) && backtracking(s,wordDict,i+1,flag)){             
                flag[index] = 1;
                return true;
            }
        }
        flag[index] = 0;
        return false;
    }
}

class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int n = s.length();
        int[] dp = new int[n+1];
        dp[0] = 1;
        for(int i=1;i<=n;i++){
            for(int j=0;j<i;j++){
                String temp = s.substring(j,i);
                if(wordDict.contains(temp) && dp[j]==1){
                    dp[i] = 1;
                }
            }
        }
        return dp[n]==1;
    }
}
```

# 动态规划06

## [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n<=1) return nums[0];
        int[] dp = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0],nums[1]);
        for(int i=2;i<n;i++){
            dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[n-1];
    }
}
```

## [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n==0) return 0;
        if(n==1) return nums[0];
        int res1 = fun(nums,0,n-2);
        int res2 = fun(nums,1,n-1);
        return Math.max(res1,res2);
    }

    public int fun(int[] nums,int start,int end){
        if(start==end) return nums[start];
        int[] dp = new int[end+1];
        dp[start] = nums[start];
        dp[start+1] = Math.max(nums[start],nums[start+1]);
        for(int i=start+2;i<=end;i++){
            dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[end];
    }
}
```

## [337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

//错误的思路
class Solution {
    public int rob(TreeNode root) {
        if(root==null) return 0;
        if(root.left==null && root.right==null) return root.val;
        ArrayList<Integer> list = layerOrder(root);
        int[] dp = new int[list.size()];
        dp[0] = list.get(0);
        dp[1] = Math.max(list.get(0),list.get(1));
        for(int i=2;i<list.size();i++){
            dp[i] = Math.max(dp[i-1],dp[i-2]+list.get(i));
        }
        return dp[list.size()-1];
    }

    public ArrayList layerOrder(TreeNode root){
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        ArrayList<Integer> list = new ArrayList<>();
        while(!q.isEmpty()){
            int s = q.size();
            int r = 0;
            for(int i=0;i<s;i++){
                TreeNode t = q.poll();
                r += t.val;
                if(t.left!=null) q.offer(t.left);
                if(t.right!=null) q.offer(t.right);
            }
            list.add(r);
            System.out.println(r);
        }
        return list;
    }
}

//超时
class Solution {
    public int rob(TreeNode root) {
        if(root==null) return 0;
        if(root.left==null && root.right==null) return root.val;
        int res1 = 0;
        res1 += root.val;
        if(root.left!=null) res1 = res1 + rob(root.left.left) + rob(root.left.right);
        if(root.right!=null) res1 = res1 + rob(root.right.left) + rob(root.right.right);
        int res2 = 0 ;
        res2 = res2 + rob(root.left) + rob(root.right);
        return Math.max(res1,res2);
    }
}

//记忆化递归
class Solution {
    Map<TreeNode,Integer> map = new HashMap<>();
    public int rob(TreeNode root) {
        if(root==null) return 0;
        if(root.left==null && root.right==null) return root.val;
        if(map.containsKey(root)) return map.get(root);
        int res1 = 0;
        res1 += root.val;
        if(root.left!=null) res1 = res1 + rob(root.left.left) + rob(root.left.right);
        if(root.right!=null) res1 = res1 + rob(root.right.left) + rob(root.right.right);
        int res2 = 0 ;
        res2 = res2 + rob(root.left) + rob(root.right);
        int res = Math.max(res1,res2);
        map.put(root,res);
        return res;
    }
}

class Solution {
    public int rob(TreeNode root) {
    int[] result = robInternal(root);
    return Math.max(result[0], result[1]);
}

public int[] robInternal(TreeNode root) {
    if (root == null) return new int[2];
    int[] result = new int[2];

    int[] left = robInternal(root.left);
    int[] right = robInternal(root.right);

    result[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
    result[1] = left[0] + right[0] + root.val;

    return result;
}
}
```

## 121. 买卖股票的最佳时机

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        for(int i=1;i<n;i++){
            dp[i][0] = Math.max(dp[i-1][0],-prices[i]);
            dp[i][1] = Math.max(dp[i-1][1],dp[i-1][0]+prices[i]);
        }
        return dp[n-1][1];
    }
}
```

## [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        for(int i=1;i<n;i++){
            dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]-prices[i]);
            dp[i][1] = Math.max(dp[i-1][1],dp[i-1][0]+prices[i]);
        }
        return dp[n-1][1];
    }
}
```

## [123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][5];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        dp[0][2] = 0;
        dp[0][3] = -prices[0];
        dp[0][4] = 0;
        for(int i=1;i<n;i++){
            dp[i][0] = dp[i-1][0];
            dp[i][1] = Math.max(dp[i-1][1],dp[i-1][0]-prices[i]);
            dp[i][2] = Math.max(dp[i-1][2],dp[i-1][1]+prices[i]);
            dp[i][3] = Math.max(dp[i-1][3],dp[i-1][2]-prices[i]);
            dp[i][4] = Math.max(dp[i-1][4],dp[i-1][3]+prices[i]);
        }
        return dp[n-1][4];
    }
}
/*
为什么初始化时候第二次买入利润为-price[0]?
因为，是第0天买入的，此时手头上是没有剩余利润的。能产生2次买入，可以理解为先买入在卖出，这样利润为 0；
然后再买入，这样利润就位—price[0]
*/
```

## [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        if(k==0 || n==0) return 0;
        int[][] dp = new int[n][2*k+1];
        for(int i=1;i<2*k;i+=2){
            dp[0][i] = -prices[0];
        }
        for(int i=1;i<n;i++){
            for(int j=0;j<2*k;j+=2){
                dp[i][j+1] = Math.max(dp[i-1][j+1],dp[i-1][j]-prices[i]);
                dp[i][j+2] = Math.max(dp[i-1][j+2],dp[i-1][j+1]+prices[i]);
            }
        }
        return dp[n-1][2*k];
    }
}

class Solution {
    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        if(k==0 || n==0) return 0;
        int[][] dp = new int[n][2*k+1];
        for(int i=1;i<2*k;i+=2){
            dp[0][i] = -prices[0];
        }
        for(int i=1;i<n;i++){
            for(int j=1;j<2*k;j+=2){
                dp[i][j] = Math.max(dp[i-1][j],dp[i-1][j-1]-prices[i]);
                dp[i][j+1] = Math.max(dp[i-1][j+1],dp[i-1][j]+prices[i]);
            }
        }
        return dp[n-1][2*k];
    }
}
```

## [309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][3];
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        dp[0][2] = 0;
        for(int i=1;i<n;i++){
            dp[i][0] = Math.max(dp[i-1][0],dp[i-1][2]-prices[i]);
            dp[i][1] = dp[i-1][0]+prices[i];
            dp[i][2] = Math.max(dp[i-1][2],dp[i-1][1]);
        }
        return Math.max(dp[n-1][1],dp[n-1][2]);
    }
}

class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int a = -prices[0], b = 0,c = 0;
        for(int i=1;i<n;i++){
            int new1 = Math.max(a,c-prices[i]);
            int new2 = a+prices[i];
            int new3 = Math.max(b,c);
            a = new1;
            b = new2;
            c = new3;
        }
        return Math.max(b,c);
    }
}
```

## [714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        for(int i=1;i<n;i++){
            dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]-prices[i]);
            dp[i][1] = Math.max(dp[i-1][1],dp[i-1][0]+prices[i]-fee);
        }
        return dp[n-1][1];
    }
}
```

# 动态规划07 子序列

## [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp,1);
        int res = 1;
        for(int i=1;i<n;i++){
            for(int j=0;j<i;j++){
                if(nums[i]>nums[j]){
                    dp[i] = Math.max(dp[i],dp[j]+1);
                }
            }
            res = Math.max(res,dp[i]);
        }
        return res;
    }
}

//序列
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int res = 0;
        List<List<Integer>> ans = new ArrayList<>();
        int[] dp = new int[n];
        int m = -1, v = 0;
        for(int i=0;i<n;i++){
            int index = -1;
            List<Integer> temp = new ArrayList<>();
            for(int j=0;j<i;j++){
                //从j->i哪个地方的序列最长，先记录下来
                if(nums[i]>nums[j] && dp[j]>dp[i]){
                    dp[i] = dp[j];
                    index = j;
                }
            }
            //加上i本身
            dp[i]++;
            if(index!=-1) temp.addAll(ans.get(index));
            temp.add(nums[i]);
            //ans中记录的是从i=0->n-1，所包含的最长递增序列
            //temp记录的是当前i的最长递增子序列
            ans.add(temp);
            if(dp[i]>m){
                m = dp[i];
                v = i;
            }
        }
        return res;
    }
}
```

## [674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        if(nums.length==0) return 0;
        int res = 1,m = 1;
        for(int i=1;i<nums.length;i++){
            if(nums[i]>nums[i-1])  {
                res += 1;
            }
            else{
                res = 1;
            }
            m = Math.max(m,res);
        }
        return m;
    }
}

class Solution {
    public int findLengthOfLCIS(int[] nums) {
        if(nums.length==0) return 0;
        int[] dp = new int[nums.length];
        Arrays.fill(dp,1);
        int res = 1;
        for(int i=1;i<nums.length;i++){
            if(nums[i]>nums[i-1]) dp[i] = dp[i-1] + 1;
            res = Math.max(res,dp[i]);
        }
        return res;
    }
}

//获取序列
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        if(nums.length==0) return 0;
        int res = 0;
        List<Integer> list = new ArrayList<>();
        List<Integer> ans = new ArrayList<>();
        list.add(nums[0]);
        for(int i=1;i<nums.length;i++){
            if(nums[i]>nums[i-1]){
                list.add(nums[i]);
            }
            else{
                list.clear();
                list.add(nums[i]);
            }
            if(list.size()>ans.size()){
                ans.clear();
                for(int j=0;j<list.size();j++){
                    ans.add(list.get(j));
                }
            }
        }
        System.out.println(ans);
        return res;
    }
}
```

## [718. 最长重复子数组](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)

```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int n = nums1.length, m = nums2.length;
        int[][] dp = new int[n+1][m+1];
        int res = 0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                //要求连续，所以两个序列的尾数一定要相同
                if(nums1[i-1]==nums2[j-1]){
                    //上方和左方是不行的，【10001,10011】，试试就知道了
                    //1000和1001，假如现在是3个共同的，那么10001和10011是多少个，4个么，显然错了
                    dp[i][j] = dp[i-1][j-1] + 1;
                    res = Math.max(res,dp[i][j]);
                }
            }
        }
        return res;
    }
}

class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int n = nums1.length, m = nums2.length;
        int[][] dp = new int[n][m];
        int res = 0;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(nums1[i]==nums2[j]){
                    if(i!=0 && j!=0){
                        dp[i][j] = dp[i-1][j-1] + 1;
                    }
                    else{
                        dp[i][j] = 1;
                    }
                    res = Math.max(res,dp[i][j]);
                }
            }
        }
        return res;
    }
}
```

## [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n = text1.length(), m = text2.length();
        int[][] dp = new int[n][m];
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(text1.charAt(i)==text2.charAt(j)){
                    if(i!=0 && j!=0){
                        dp[i][j] = dp[i-1][j-1] + 1;
                    }
                    else {
                        dp[i][j] = 1;
                    }
                }
                else{
                    if(i!=0 && j!=0)
                        dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]);
                    else if(i!=0)
                        dp[i][j] = dp[i-1][j];
                    else if(j!=0)
                        dp[i][j] = dp[i][j-1];
                }
                // System.out.println(i+"--"+j+"--"+dp[i][j]);
            }
            
        }
        return dp[n-1][m-1];
    }
}

class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n = text1.length(), m = text2.length();
        int[][] dp = new int[n+1][m+1];
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(text1.charAt(i-1)==text2.charAt(j-1)){
                    //dp[i][j] = Math.max(Math.max(dp[i][j-1],dp[i-1][j]),dp[i-1][j-1] + 1);
                    //但是当相等时，dp[i-1][j-1] + 1一定比其他两种情况大，或者相等，我们求得只是一个数值
                    dp[i][j] = dp[i-1][j-1] + 1;
                }
                else{
                    dp[i][j] = Math.max(dp[i][j-1],dp[i-1][j]);
                }
            } 
        }
        return dp[n][m];
    }
}
```

## [1035. 不相交的线](https://leetcode-cn.com/problems/uncrossed-lines/)

```java
class Solution {
    public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int n = nums1.length, m = nums2.length;
        int[][] dp = new int[n+1][m+1];
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(nums1[i-1]==nums2[j-1]){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }
                else{
                    dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[n][m];
    }
}
```

## [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        int m = nums[0];
        dp[0] = nums[0];
        for(int i=1;i<n;i++){
            dp[i] = Math.max(nums[i],dp[i-1]+nums[i]);
            if(dp[i]>m){
                m = dp[i];
            }
        }
        return m;
    }
}

//贪心
class Solution {
    public int maxSubArray(int[] nums) {
        int  m = Integer.MIN_VALUE, res = 0;
        for(int i=0;i<nums.length;i++){
            res += nums[i];
            m = Math.max(m,res);
            if(res<0){
                res = 0;
            }
        }
        return m;
    }
}
```

## [392. 判断子序列](https://leetcode-cn.com/problems/is-subsequence/)

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int n = s.length(), m = t.length();
        if(n==0 && m==0) return true;
        if(n==0) return true;
        int i = 0, j = 0, cnt = 0;
        for(;i<n;i++){
            for(;j<m;j++){
                if(s.charAt(i)==t.charAt(j)){
                    j++;
                    cnt++;
                    break;
                }
            }
        }
        return cnt==n;
    }
}

class Solution {
    public boolean isSubsequence(String s, String t) {
        int n = s.length(), m = t.length();
        if(n==0 && m==0) return true;
        if(n==0) return true;
        int i = 0, j = -1, cnt = 0;
        for(;i<n;i++){
            for(++j;j<m;j++){
                if(s.charAt(i)==t.charAt(j)){
                    break;
                }
            }
            if(j>=m) return false;
        }
        return true;
    }
}

//双指针
class Solution {
    public boolean isSubsequence(String s, String t) {
        int n = s.length(), m = t.length();
        if(n==0 && m==0) return true;
        if(n==0) return true;
        int v = 0;
        for(int i=0,j=0;i<m && j<n;i++){
            if(s.charAt(j)==t.charAt(i)){
                j++;
                v++;
            }
        }
        return v==n;
    }
}

class Solution {
    public boolean isSubsequence(String s, String t) {
        int n = s.length(), m = t.length();
        if(n==0 && m==0) return true;
        if(n==0) return true;
        int[][] dp = new int[n+1][m+1];
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(s.charAt(i-1)==t.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }
                else{
                    dp[i][j] = dp[i][j-1];
                }
            }
        }
        return dp[n][m]==n;
    }
}
```

