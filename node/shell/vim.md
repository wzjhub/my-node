## sed 命令

使用sed替换文件中的某一行字符时，出现的一种现象如下：

文本：vim.txt

```text
vim=#{vim}
```

shell脚本：

```shell
vim="123&123"
sed -i '/^vim s/#{vim}/"${vim}"' vim.txt
```

出现的现象：vim=123#{vim}123

在执行一次：

```shell
sed -i '/^vim s/#{vim}/\&' vim.txt
```

得到结果：vim=123&123

