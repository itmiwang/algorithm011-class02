### 学习总结

#### Week03

#### 知识点总结

##### 递归的实现与特性

```
//递归代码模板
public void recur(int level, int param) { 
	// terminator 递归终结条件
    if (level > MAX_LEVEL) { 
		// process result 
		return; 
	}
	// process current logic 处理当前层逻辑
	process(level, param); 
	// drill down 下探到下一层
	recur( level: level + 1, newParam); 
	// restore current status 清理当前层
}

//三个思维要点
1、抵制人肉递归
2、找最近重复性
3、数学归纳法思维
```

------

##### 分治、回溯的实现与特性

```
//分治
化解成若干个子问题进行解决，组合子结果返回，重点在于重复性

//回溯 （试错思想，分步尝试）
1、路径：也就是已经做出的选择。
2、选择列表：也就是你当前可以做的选择。
3、结束条件：也就是到达决策树底层，无法再做选择的条件
backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    for 选择 in 选择列表:
        # 做选择
        将该选择从选择列表移除
        路径.add(选择)
        backtrack(路径, 选择列表)
        # 撤销选择
        路径.remove(选择)
        将该选择再加入选择列表
```

------

##### 日常总结

1、不断尝试平衡工作和学习提升的时间

