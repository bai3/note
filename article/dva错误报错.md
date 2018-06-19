## dva项目webpack修改后导致错误

dva的配置基于roadhog，修改webpack后不知道为什么就报错。

### 报错

当我运行cnpm run dev

> ```
> Failed to start the server, since you have enabled dllPlugin, but have not run `roadhog buildDll` before `roadhog server`.
> ```

### 解决

google找到dva相应的issue提问，解决方法如下，实测可用

>1. cnpm i roadhog -g
>2. roadhog build
>3. cnpm start

### 另外问题

修改后，cnpm可运行，可是某一文件报错说找不到路径，其实路径存在文件。

类似以下报错：

>  [Module not found: [CaseSensitivePathsPlugin\] `…\react.js` does not match the corresponding path on disk `react`

#### 解决方法

删除文件，重新创建文件，并复制内容，实测可用。