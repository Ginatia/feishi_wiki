# 行为树

## sequence

定义若干个子节点，按顺序执行所有节点。

若遇到节点失败，则退出。

此模式适合定义具有前后依赖的行为。

行为树定义如下：

```
advanced_behavior_tree
         btt on
         sequence
            behavior_node checkpoint_1
            behavior_node checkpoint_2
            behavior_node checkpoint_3
            behavior_node checkpoint_4
            behavior_node checkpoint_5
         end_sequence
end_advanced_behavior_tree
```

行为节点定义如下：

高级行为树可以返回三种状态，分别为：

`Running,Success,Failure`

```
advanced_behavior checkpoint_1
   
   # 定义变量
   script_variables
      bool commandIssued = false;
   end_script_variables

   # 每个节点必须有前置条件脚本块，用于确定该行为是否执行
   precondition
      if (SharedBlackboardVarExists("checkpoint_tasks", "checkpoint_1"))
      {
         return Success();
      }
      else
      {
         return Failure("No checkpoint_1 task found!");
      }
   end_precondition
   
   # 执行脚本块
   execute
      if (PLATFORM.GroundRangeTo(WsfSimulation.FindPlatform("Blue_checkpoint_1")) < 10000)
      {
         DeleteSharedBlackboardVar("checkpoint_tasks", "checkpoint_1");
         return Success("Checkpoint 1 reached!");
      }
      
      if (!commandIssued)
      {
         commandIssued = true;
         FlyTarget(PLATFORM, WsfSimulation.FindPlatform("Blue_checkpoint_1").Location(), 450);
      }
      return Running("Flying to Checkpoint 1");
   end_execute
   
end_advanced_behavior
```


此行为节点执行块：

若在一定距离内与检查点接近，则返回 Success

若未接近检查点，且还未执行向检查点机动，则执行机动，返回 Running

其余行为节点类似，只是各检查点不一样。

## select

