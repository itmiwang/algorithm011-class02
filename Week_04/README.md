### 学习总结

#### Week04

#### 知识点总结

##### 深度优先搜索和广度优先搜索的实现和特性

```java
//DFS模板
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> allResults = new ArrayList<>();
    if(root==null){
        return allResults;
    }
    travel(root,0,allResults);
    return allResults;
}


private void travel(TreeNode root,int level,List<List<Integer>> results){
    if(results.size()==level){
        results.add(new ArrayList<>());
    }
    results.get(level).add(root.val);
    if(root.left!=null){
        travel(root.left,level+1,results);
    }
    if(root.right!=null){
        travel(root.right,level+1,results);
    }
}


//BFS模板
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}

public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> allResults = new ArrayList<>();
    if (root == null) {
        return allResults;
    }
    Queue<TreeNode> nodes = new LinkedList<>();
    nodes.add(root);
    while (!nodes.isEmpty()) {
        int size = nodes.size();
        List<Integer> results = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            TreeNode node = nodes.poll();
            results.add(node.val);
            if (node.left != null) {
                nodes.add(node.left);
            }
            if (node.right != null) {
                nodes.add(node.right);
            }
        }
        allResults.add(results);
    }
    return allResults;
}
```

##### 贪心算法的实现和特性

```
贪心：当下做局部最优判断
回溯：能够回退
动态规划：最优判断 + 回退
```

##### 二分查找的实现和特性

```
二分查找的前提：
1、目标函数单调性（单调递增或者递减）
2、存在上下界（bounded）
3、能够通过索引访问（index accessible）
```

```java
//二分查找模板
public int binarySearch(int[] array, int target) {
    int left = 0, right = array.length - 1, mid;
    while (left <= right) {
        mid = (right - left) / 2 + left;

        if (array[mid] == target) {
            return mid;
        } else if (array[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return -1;
}
```

------

#### 日常总结

1、本周的课程内容个人感觉更加侧重于对问题的分析和解决方法的匹配方面，且DFS和BFS以及二分查找的模板要熟练掌握，以便于提升解题速度

2、No.33 搜索旋转排序数组

```java
//注意判断边界的等号使用
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[left] <= nums[mid]) {
                //mid左侧有序，右侧无序
                if (nums[left] <= target && nums[mid] > target) {
                    //target可能在mid左侧有序中存在，调整右边界
                    right = mid - 1;
                } else {
                    //target可能在mid右侧无序中存在
                    left = mid + 1;
                }
            } else {
                //mid左侧无序，右侧有序
                if (nums[mid] < target && nums[right] >= target) {
                    //target可能在mid右侧有序中存在，调整左边界
                    left = mid + 1;
                } else {
                    //target可能在mid左侧无序中存在，调整右边界
                    right = mid - 1;
                }
            }
        }
        return -1;
    }
}
```

------

##### 

