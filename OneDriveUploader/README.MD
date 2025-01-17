# OneDriveUploader
- 支持上传文件和文件夹到指定目录,并保持上传前的目录结构.
- 支持命令参数使用, 方便外部程序调用.
- 支持自定义上传分块大小.
- 支持多线程上传(多文件同时上传).
- 支持根据文件大小动态调整重试次数

## 授权
### 通过下面URL登录 (右键新标签打开)
[https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=ea2b36f6-b8ad-40be-bc0f-e5e4a4a7d4fa&response_type=code&redirect_uri=http://localhost/onedrive-login&response_mode=query&scope=offline_access%20User.Read%20Files.ReadWrite.All](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=ea2b36f6-b8ad-40be-bc0f-e5e4a4a7d4fa&response_type=code&redirect_uri=http://localhost/onedrive-login&response_mode=query&scope=offline_access%20User.Read%20Files.ReadWrite.All)

### 初始化配置文件
```
# 运行
OneDriveUploader -a "url"

# 在浏览器地址栏中获取以 http://loaclhost 开头的整个url内容
# 将获取的完整url内容替换命令中的 url 三个字母
# 此操作将会自动初始化的配置文件,只需要操作一次
# 提示 Init config file: '/path/to/auth.json' 则成功
```

## 使用
```
Usage of OneDriveUploader:
  -a string
        // 初始化授权
        Setup and Init auth.json.
  -b string
        // 自定义上传分块大小, 可以提高网络吞吐量, 受限于磁盘性能和网络速度.
        Set block size. [Unit: M; 5<=b<=60;] (default "10")
  -c string
        // 配置文件路径
        Config file. (default "auth.json")
  -f    Force Read config form config file. [BlockSize, ThreadNum]
        // 开关
        // 加上 -f 参数，强制读取 auth.json 中的块大小配置和多线程配置.
        // 不加 -f 参数, 每次覆盖保存当前使用参数到 auth.json 配置文件中.
  -n string
        // 上传单个文件时,在网盘中重命名
        Rename file on upload to remote.
  -r string
        // 上传到网盘中的某个目录, 默认: 根目录
        Upload to reomte path.
  -s string
        // *必要参数, 要上传的文件或文件夹
        Upload item.
  -t string
        // 线程数, 同时上传文件的个数. 默认: 2
        Set thread num. (default "2")
        
```

## 示例
```
# 一些示例:
# 将同目录下的 mm00.jpg 文件上传到 OneDrive 网盘根目录
OneDriveUploader -s "mm00.jpg"

# 将同目录下的 mm00.jpg 文件上传到 OneDrive 网盘根目录,并改名为 mm01.jpg
OneDriveUploader -s "mm00.jpg" -n "mm01.jpg"

# 将同目录下的 Download 文件夹上传到 OneDrive 网盘根目录
OneDriveUploader -s "Download" 

# 将同目录下的 Download 文件夹上传到 OneDrive 网盘Test目录中
OneDriveUploader -s "Download" -r "Test"

# 将同目录下的 Download 文件夹上传到 OneDrive 网盘Test目录中, 使用 10 线程
OneDriveUploader -t 10 -s "Download" -r "Test"

# 将同目录下的 Download 文件夹上传到 OneDrive 网盘Test目录中, 使用 15 线程, 并设置分块大小为 20M
OneDriveUploader -t 15 -b 20 -s "Download" -r "Test"

# 将同目录下的 Download 文件夹上传到 OneDrive 网盘Test目录中, 使用配置文件中的线程参数和分块大小参数
OneDriveUploader -f -c "/urs/local/auth.json" -s "Download" -r "Test"
```

## 注意
- 多次尝试后, 无失败的上传文件. 退出码为 0 .
- 最终还有失败的上传文件会详细列出上传失败项. 退出码为 1.
