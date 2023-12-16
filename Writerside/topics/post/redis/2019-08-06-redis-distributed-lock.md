---
title: 关于redis分布式锁入门
Published: 08/06/2016
tags: ['redis','原理','分布式锁']
---

# 原理

单一实例下：使用setnx命令(在key不存在时,创建并设置value 返回1,key存在时,会返回0)来获取锁。

# 应用

以扣库存为例，使用锁能够有效保证不会被超卖；

- 基础版方案：数据库锁

  ```
  SELECT stock from goods where ID =1 for update;
  update  goods set stock=stock-1 where id=1 and stock=10
  // 正常情况下看你业务需求，如果能够在落库前确认库存的话，上面这句话后面的stock=10能够限定库存锁定值
  // 如果不使用这个stock=10的话，会有一定的副作用，比如说常见的，多线程轮扣
  ```

- ## redis 版v0

​ 使用setnx写入一个键值，如果成功则表明获取到锁，执行业务;对于没有获取到锁的，则会轮询获取。

```
            int waitIntervalMs = 50;//间隔等待时长 毫秒
            string lockKey = "lock_key:" + key; 
            DateTime begin = DateTime.Now;
            while (true)
            {
                if (redisClient.SetNX(lockKey, new byte[] { 1 }) == 1)
                {
                    redisClient.Expire(lockKey, expirySeconds);
                    return true;
                } 
                //不等待锁则返回
                if (waitSeconds <= 0)
                    break;

                if ((DateTime.Now - begin).TotalSeconds >= waitSeconds)//等待超时
                    break;

                System.Threading.Thread.Sleep(waitIntervalMs);
            }
            return false;
```

**不管是该业务成功、失败或者异常，需要确保该锁在周期结束后能被释放**

- redis 版v1

  V0版本存在的缺陷：比较致命的是可能会出现，任务还未完成，锁就超时了，导致key被回收了，那么能不能将锁的申请时间作为值进行存储呢？

  ```
  ​```
  int waitIntervalMs = 200;//间隔等待时长 毫秒 设置成足够长，至少要超过数据库的超时响应时间
  string lockKey = "lock_key:" + key; 
  DateTime begin = DateTime.Now;
  DateTime now = DateTime.Now;
  while (true)
              {
                //在key值存在的情况下
                 if(redisclient.ExistKey(lockKey))
                 {
                 //如果key对应的value值+超时时间后小于当前时间，则移除key值，让业务正常运行
                 if(Convert.ToDateTime(redisclient.GetKey(lockkey).Value)+TimeLimit<now){
                 redisclient.remove(lockkey);
                 }
                 }
                  if (redisClient.SetNX(lockKey, now.tostring()) == 1)
                  {
                      redisClient.Expire(lockKey, expirySeconds);
                      return true;
                  } 
                  //不等待锁则返回
                  if (waitSeconds <= 0)
                      break;
  
                  if ((DateTime.Now - begin).TotalSeconds >= waitSeconds)//等待超时
                      break;
  
                  System.Threading.Thread.Sleep(waitIntervalMs);
              
              
              }
              return false; 
  ```

  # 遗留的问题

  在单个redis实例下，上述代码工作都ok，如果存在redis集群，且master挂掉的情况，那么这个锁一定锁不住
