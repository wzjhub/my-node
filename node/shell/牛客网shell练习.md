https://www.nowcoder.com/ta/shell

| 题号    | 题目                         | 难度 | 通过率 |
| ------- | ---------------------------- | ---- | ------ |
| SHELL1  | 统计文件的行数               | 简单 | 43.08% |
| SHELL2  | 打印文件的最后5行            | 简单 | 58.99% |
| SHELL3  | 输出7的倍数                  | 中等 | 31.25% |
| SHELL4  | 输出第5行的内容              | 中等 | 50.43% |
| SHELL5  | 打印空行的行号               | 中等 | 35.66% |
| SHELL6  | 去掉空行                     | 中等 | 50.29% |
| SHELL7  | 打印字母数小于8的单词        | 较难 | 30.84% |
| SHELL8  | 统计所有进程占用内存大小的和 | 较难 | 39.11% |
| SHELL9  | 统计每个单词出现的个数       | 较难 | 35.33% |
| SHELL10 | 第二列是否有重复             | 较难 | 36.84% |
| SHELL11 | 转置文件的内容               | 较难 | 41.54% |
| SHELL12 | 打印每一行出现的数字个数     | 较难 | 30.14% |
| SHELL13 | 去掉所有包含this的句子       | 中等 | 58.28% |
| SHELL14 | 求平均值                     | 较难 | 26.33% |
| SHELL15 | 去掉不需要的单词             | 较难 | 55.29% |

##  SHELL1  统计文件的行数

```shell
awk 'END{print NR}' nowcoder.txt
```

```shell
cat nowcoder.txt |wc -l
```

##  SHELL2  打印文件的最后5行

```shell
tail -n 5 nowcoder.txt
```

```shell
tail -5 nowcoder.txt
```

##  SHELL3  输出7的倍数

```shell
limit=0
while true
do
    now=`expr 7 \* $limit`
    #echo -n "now: " && echo `expr 7 \* $limit`
    [ $now -le 500 ] && echo $now
    [ $now -gt 500 ] && break
    let limit+=1
done
```

## SHELL4  输出第5行的内容

```shell
sed -n '5p' nowcoder.txt
```

```shell
echo `awk 'NR==5 {print}' nowcoder.txt`
```

##  SHELL5  打印空行的行号

```shell
awk '/^$/ {print NR}' nowcoder.txt
```

```shell
sed -n '/^$/=' nowcoder.txt
```

## SHELL6  去掉空行

```shell
awk NF nowcoder.txt
```

##  SHELL7  打印字母数小于8的单词

```shell
for i in `cat nowcoder.txt`
do
    if [[ ${#i} -lt 8 ]];then
      echo $i
    fi
done
```

##  SHELL8  统计所有进程占用内存大小的和

##  SHELL9  统计每个单词出现的个数

##  SHELL10  第二列是否有重复