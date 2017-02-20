---
title: 生命游戏
date: 2017-02-20 20:47:34
tags:
---

## 规则

每个格子的生死遵循下面的原则：

如果一个细胞周围有3个细胞为生（一个细胞周围共有8个细胞），则该细胞为生（即该细胞若原先为死，则转为生，若原先为生，则保持不变） 。

如果一个细胞周围有2个细胞为生，则该细胞的生死状态保持不变；

在其它情况下，该细胞为死（即该细胞若原先为生，则转为死，若原先为死，则保持不变）

---

## 代码

```javascript
    /**
    * @desc 求周围的存活的细胞个数
    * @param map 当前细胞地图
    * @param x y 当前细胞位置
    */
    round: (map,x,y) =>{
        let count = 0
        map.forEach((row,i) =>{
            row.forEach((col,j) =>{
                if((Math.abs(i-x) == 1 || Math.abs(j-y) == 1) 
                    && Math.abs(i-x) < 2 && Math.abs(j-y) < 2)
                    count += map[i][j] ? map[i][j] : 0
            })
        })
        return count
    }

    /**
     * @desc 返回一个细胞下一时间的状态
     * @param status 细胞当前状态 0:死 1:活
     * @param count 细胞当前周围的存活细胞数量
     */
    nextCellStatus: (status,count) =>{
        if(count == 3) return 1
        if(count == 2) return status
        return 0
    }

    /**
     * @desc 生成下一时刻的地图
     * @param 当前时刻的map
     * @return 下一时刻的map
     */
    nextMap: (map) =>{
        let newmap = map.map(row => row.map(col => 0))
        map.map((row,i) =>{
            row.map((col,j) =>{
                const count = round(map,i,j)
                newmap[i][j] = nextCellStatus(map[i][j],count)
            })
        })
        return newmap
    }
```

---

## 其他

我已经把这个代码封装成一个npm包。通过 `npm install lifegame` 即可安装。

[npm地址](https://www.npmjs.com/package/lifegame)

[github地址](https://github.com/moyunchen/lifegame)

有时间我会写一篇关于如何写一个npm包,然后在github上集成travis-ci进行自动化测试以及codecov进行代码覆盖率测试。